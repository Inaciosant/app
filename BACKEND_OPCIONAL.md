# 🌐 Backend API (Opcional) - Dart/Shelf

Esta é uma implementação opcional de backend em Dart usando o package Shelf. O aplicativo funciona perfeitamente offline com SQLite local, mas este backend pode ser usado para:

- Sincronização entre dispositivos
- Backup na nuvem  
- Funcionalidades colaborativas

## 📁 Estrutura do Backend

```
backend/
├── bin/
│   └── server.dart
├── lib/
│   ├── handlers/
│   │   ├── expense_handler.dart
│   │   └── category_handler.dart
│   ├── models/
│   │   ├── expense.dart
│   │   └── category.dart
│   ├── database/
│   │   └── database.dart
│   └── middleware/
│       └── cors_middleware.dart
└── pubspec.yaml
```

## 🚀 Instalação do Backend

### 1. Criar novo projeto Dart
```bash
dart create -t server-shelf backend
cd backend
```

### 2. Adicionar dependências
```yaml
# pubspec.yaml
dependencies:
  shelf: ^1.4.1
  shelf_router: ^1.1.4
  shelf_cors_headers: ^0.1.5
  sqlite3: ^2.1.0
  json_annotation: ^4.8.1

dev_dependencies:
  build_runner: ^2.4.6
  json_serializable: ^6.7.1
```

### 3. Instalar dependências
```bash
dart pub get
```

## 📝 Código do Backend

### Server Principal (bin/server.dart)
```dart
import 'dart:io';
import 'package:shelf/shelf.dart';
import 'package:shelf/shelf_io.dart';
import 'package:shelf_router/shelf_router.dart';
import 'package:shelf_cors_headers/shelf_cors_headers.dart';
import '../lib/handlers/expense_handler.dart';
import '../lib/handlers/category_handler.dart';
import '../lib/database/database.dart';

void main(List<String> args) async {
  final database = Database();
  await database.init();
  
  final expenseHandler = ExpenseHandler(database);
  final categoryHandler = CategoryHandler(database);
  
  final router = Router()
    // Expenses
    ..get('/expenses', expenseHandler.getAll)
    ..get('/expenses/<id>', expenseHandler.getById)
    ..post('/expenses', expenseHandler.create)
    ..put('/expenses/<id>', expenseHandler.update)
    ..delete('/expenses/<id>', expenseHandler.delete)
    
    // Categories
    ..get('/categories', categoryHandler.getAll)
    ..post('/categories', categoryHandler.create)
    ..put('/categories/<id>', categoryHandler.update)
    ..delete('/categories/<id>', categoryHandler.delete)
    
    // Statistics
    ..get('/stats/total', expenseHandler.getTotal)
    ..get('/stats/monthly', expenseHandler.getMonthlyTotal)
    ..get('/stats/by-category', expenseHandler.getByCategory);

  final handler = Pipeline()
      .addMiddleware(corsHeaders())
      .addMiddleware(logRequests())
      .addHandler(router);

  final ip = InternetAddress.anyIPv4;
  final port = int.parse(Platform.environment['PORT'] ?? '8080');
  final server = await serve(handler, ip, port);
  
  print('Server listening on port \${server.port}');
}
```

### Database Helper (lib/database/database.dart)
```dart
import 'package:sqlite3/sqlite3.dart';

class Database {
  late sqlite3.Database _db;
  
  Future<void> init() async {
    _db = sqlite3.open('expense_manager.db');
    
    _db.execute('''
      CREATE TABLE IF NOT EXISTS expenses (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        title TEXT NOT NULL,
        amount REAL NOT NULL,
        category TEXT NOT NULL,
        date INTEGER NOT NULL,
        description TEXT
      )
    ''');
    
    _db.execute('''
      CREATE TABLE IF NOT EXISTS categories (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL UNIQUE,
        icon TEXT NOT NULL,
        color TEXT NOT NULL
      )
    ''');
  }
  
  sqlite3.Database get db => _db;
  
  void close() {
    _db.dispose();
  }
}
```

### Expense Handler (lib/handlers/expense_handler.dart)
```dart
import 'dart:convert';
import 'package:shelf/shelf.dart';
import '../database/database.dart';
import '../models/expense.dart';

class ExpenseHandler {
  final Database database;
  
  ExpenseHandler(this.database);
  
  Response getAll(Request request) {
    try {
      final results = database.db.select('SELECT * FROM expenses ORDER BY date DESC');
      final expenses = results.map((row) => Expense.fromMap(row)).toList();
      
      return Response.ok(
        json.encode(expenses.map((e) => e.toMap()).toList()),
        headers: {'Content-Type': 'application/json'},
      );
    } catch (e) {
      return Response.internalServerError(body: json.encode({'error': e.toString()}));
    }
  }
  
  Response getById(Request request) {
    final id = int.tryParse(request.params['id']!);
    if (id == null) {
      return Response.badRequest(body: json.encode({'error': 'Invalid ID'}));
    }
    
    try {
      final results = database.db.select('SELECT * FROM expenses WHERE id = ?', [id]);
      if (results.isEmpty) {
        return Response.notFound(json.encode({'error': 'Expense not found'}));
      }
      
      final expense = Expense.fromMap(results.first);
      return Response.ok(
        json.encode(expense.toMap()),
        headers: {'Content-Type': 'application/json'},
      );
    } catch (e) {
      return Response.internalServerError(body: json.encode({'error': e.toString()}));
    }
  }
  
  Future<Response> create(Request request) async {
    try {
      final body = json.decode(await request.readAsString());
      final expense = Expense.fromMap(body);
      
      final result = database.db.execute(
        'INSERT INTO expenses (title, amount, category, date, description) VALUES (?, ?, ?, ?, ?)',
        [expense.title, expense.amount, expense.category, expense.date.millisecondsSinceEpoch, expense.description],
      );
      
      return Response.ok(
        json.encode({'id': database.db.lastInsertRowId, 'message': 'Expense created'}),
        headers: {'Content-Type': 'application/json'},
      );
    } catch (e) {
      return Response.badRequest(body: json.encode({'error': e.toString()}));
    }
  }
  
  Future<Response> update(Request request) async {
    final id = int.tryParse(request.params['id']!);
    if (id == null) {
      return Response.badRequest(body: json.encode({'error': 'Invalid ID'}));
    }
    
    try {
      final body = json.decode(await request.readAsString());
      final expense = Expense.fromMap({...body, 'id': id});
      
      database.db.execute(
        'UPDATE expenses SET title = ?, amount = ?, category = ?, date = ?, description = ? WHERE id = ?',
        [expense.title, expense.amount, expense.category, expense.date.millisecondsSinceEpoch, expense.description, id],
      );
      
      return Response.ok(
        json.encode({'message': 'Expense updated'}),
        headers: {'Content-Type': 'application/json'},
      );
    } catch (e) {
      return Response.badRequest(body: json.encode({'error': e.toString()}));
    }
  }
  
  Response delete(Request request) {
    final id = int.tryParse(request.params['id']!);
    if (id == null) {
      return Response.badRequest(body: json.encode({'error': 'Invalid ID'}));
    }
    
    try {
      database.db.execute('DELETE FROM expenses WHERE id = ?', [id]);
      return Response.ok(
        json.encode({'message': 'Expense deleted'}),
        headers: {'Content-Type': 'application/json'},
      );
    } catch (e) {
      return Response.internalServerError(body: json.encode({'error': e.toString()}));
    }
  }
  
  Response getTotal(Request request) {
    try {
      final result = database.db.select('SELECT SUM(amount) as total FROM expenses');
      final total = result.first['total'] ?? 0;
      
      return Response.ok(
        json.encode({'total': total}),
        headers: {'Content-Type': 'application/json'},
      );
    } catch (e) {
      return Response.internalServerError(body: json.encode({'error': e.toString()}));
    }
  }
  
  Response getMonthlyTotal(Request request) {
    try {
      final now = DateTime.now();
      final startOfMonth = DateTime(now.year, now.month, 1);
      final endOfMonth = DateTime(now.year, now.month + 1, 0, 23, 59, 59);
      
      final result = database.db.select(
        'SELECT SUM(amount) as total FROM expenses WHERE date BETWEEN ? AND ?',
        [startOfMonth.millisecondsSinceEpoch, endOfMonth.millisecondsSinceEpoch],
      );
      
      final total = result.first['total'] ?? 0;
      
      return Response.ok(
        json.encode({'monthly_total': total}),
        headers: {'Content-Type': 'application/json'},
      );
    } catch (e) {
      return Response.internalServerError(body: json.encode({'error': e.toString()}));
    }
  }
  
  Response getByCategory(Request request) {
    try {
      final results = database.db.select(
        'SELECT category, SUM(amount) as total FROM expenses GROUP BY category'
      );
      
      final categoryTotals = <String, double>{};
      for (final row in results) {
        categoryTotals[row['category']] = row['total'];
      }
      
      return Response.ok(
        json.encode(categoryTotals),
        headers: {'Content-Type': 'application/json'},
      );
    } catch (e) {
      return Response.internalServerError(body: json.encode({'error': e.toString()}));
    }
  }
}
```

## 🚀 Executar o Backend

```bash
# Desenvolvimento
dart run bin/server.dart

# Ou com hot reload
dart run --enable-vm-service bin/server.dart

# Produção
dart compile exe bin/server.dart -o server
./server
```

## 🌍 Endpoints da API

### Despesas
- `GET /expenses` - Listar todas as despesas
- `GET /expenses/{id}` - Obter despesa por ID
- `POST /expenses` - Criar nova despesa
- `PUT /expenses/{id}` - Atualizar despesa
- `DELETE /expenses/{id}` - Excluir despesa

### Categorias
- `GET /categories` - Listar todas as categorias
- `POST /categories` - Criar nova categoria
- `PUT /categories/{id}` - Atualizar categoria
- `DELETE /categories/{id}` - Excluir categoria

### Estatísticas
- `GET /stats/total` - Total geral de gastos
- `GET /stats/monthly` - Total do mês atual
- `GET /stats/by-category` - Gastos por categoria

## 📱 Integração com Flutter

Para integrar o backend com o app Flutter, você pode usar o package `http`:

```yaml
# pubspec.yaml
dependencies:
  http: ^1.1.0
```

```dart
// lib/services/api_service.dart
import 'package:http/http.dart' as http;
import 'dart:convert';

class ApiService {
  static const String baseUrl = 'http://localhost:8080'; // ou seu servidor
  
  static Future<List<Expense>> getExpenses() async {
    final response = await http.get(Uri.parse('\$baseUrl/expenses'));
    if (response.statusCode == 200) {
      final List<dynamic> data = json.decode(response.body);
      return data.map((e) => Expense.fromMap(e)).toList();
    }
    throw Exception('Failed to load expenses');
  }
  
  static Future<void> createExpense(Expense expense) async {
    await http.post(
      Uri.parse('\$baseUrl/expenses'),
      headers: {'Content-Type': 'application/json'},
      body: json.encode(expense.toMap()),
    );
  }
  
  // Outros métodos...
}
```

## 🐳 Deploy com Docker

```dockerfile
FROM dart:stable AS build

WORKDIR /app
COPY pubspec.* ./
RUN dart pub get

COPY . .
RUN dart compile exe bin/server.dart -o server

FROM scratch
COPY --from=build /runtime/ /
COPY --from=build /app/server /app/

EXPOSE 8080
ENTRYPOINT ["/app/server"]
```

## ⚙️ Configuração de Ambiente

```bash
# .env
PORT=8080
DATABASE_URL=sqlite:expense_manager.db
```

**Nota:** Este backend é completamente opcional. O aplicativo Flutter funciona perfeitamente offline com SQLite local!
