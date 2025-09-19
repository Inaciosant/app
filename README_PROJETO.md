# Gerenciador de Despesas Pessoais

Um aplicativo mobile desenvolvido em Flutter para controle e acompanhamento de despesas pessoais.

## 📱 Funcionalidades

- ✅ **CRUD Completo de Despesas**: Criar, visualizar, editar e excluir despesas
- ✅ **Gerenciamento de Categorias**: Criar e personalizar categorias com ícones e cores
- ✅ **Dashboard com Estatísticas**: Visualizar totais, gastos por categoria e estatísticas
- ✅ **Armazenamento Local**: Dados salvos localmente usando SQLite
- ✅ **Interface Intuitiva**: Design moderno e fácil de usar
- ✅ **Filtros e Buscas**: Filtrar despesas por categoria e período
- ✅ **Relatórios**: Estatísticas detalhadas de gastos

## 🏗️ Arquitetura

O projeto está estruturado da seguinte forma:

```
lib/
├── database/
│   └── database_helper.dart     # Gerenciamento do banco SQLite
├── models/
│   ├── expense.dart            # Modelo de dados para despesas
│   └── category.dart           # Modelo de dados para categorias
├── screens/
│   ├── home_screen.dart        # Tela principal com lista de despesas
│   ├── add_expense_screen.dart # Tela para adicionar/editar despesas
│   ├── expense_details_screen.dart # Detalhes da despesa
│   ├── statistics_screen.dart  # Tela de estatísticas
│   └── categories_screen.dart  # Gerenciar categorias
└── main.dart                   # Entrada do aplicativo
```

## 🚀 Como executar

### Pré-requisitos
- Flutter SDK (versão 3.8.1 ou superior)
- Dart SDK
- Android Studio / VS Code
- Emulador Android ou dispositivo físico

### Passos para execução

1. **Clone o repositório ou use o projeto atual**

2. **Instale as dependências**
   ```bash
   flutter pub get
   ```

3. **Execute o aplicativo**
   ```bash
   flutter run
   ```

## 📦 Dependências Principais

- `sqflite`: Para armazenamento local (SQLite)
- `path`: Para gerenciar caminhos de arquivos
- `intl`: Para formatação de datas e números
- `flutter_material_color_picker`: Para seleção de cores
- `flutter_colorpicker`: Para picker de cores avançado

## 🎨 Funcionalidades Detalhadas

### 1. Gerenciamento de Despesas
- **Adicionar**: Título, valor, categoria, data e descrição opcional
- **Editar**: Modificar qualquer campo da despesa
- **Visualizar**: Detalhes completos com opções de edição/exclusão
- **Excluir**: Com confirmação para evitar exclusões acidentais

### 2. Categorias Personalizáveis
- **Categorias Padrão**: Alimentação, Transporte, Saúde, Educação, etc.
- **Criar Novas**: Nome personalizado, emoji/ícone e cor
- **Editar/Excluir**: Gerenciamento completo das categorias

### 3. Estatísticas e Relatórios
- **Total Geral**: Soma de todas as despesas
- **Gastos Mensais**: Total do mês atual
- **Por Categoria**: Distribuição percentual dos gastos
- **Média Diária**: Cálculo automático baseado no mês atual
- **Despesas Recentes**: Últimas 5 transações

### 4. Interface do Usuário
- **Design Material 3**: Interface moderna e intuitiva
- **Cores Temáticas**: Esquema de cores verde para finanças
- **Navegação Fluida**: Transições suaves entre telas
- **Responsivo**: Funciona em diferentes tamanhos de tela

## 🗃️ Estrutura do Banco de Dados

### Tabela `expenses`
- `id`: INTEGER PRIMARY KEY
- `title`: TEXT NOT NULL
- `amount`: REAL NOT NULL  
- `category`: TEXT NOT NULL
- `date`: INTEGER NOT NULL (timestamp)
- `description`: TEXT (opcional)

### Tabela `categories`
- `id`: INTEGER PRIMARY KEY
- `name`: TEXT NOT NULL UNIQUE
- `icon`: TEXT NOT NULL
- `color`: TEXT NOT NULL (código hex)

## 🔧 Personalização

### Adicionar Novas Funcionalidades
1. **Backup/Restore**: Implementar exportação/importação de dados
2. **Gráficos**: Adicionar gráficos com bibliotecas como `fl_chart`
3. **Orçamento**: Sistema de metas e limites por categoria
4. **Notificações**: Lembretes para registrar gastos
5. **Sincronização**: Backup em nuvem com Firebase

### Temas e Cores
- Modificar `ThemeData` em `main.dart`
- Personalizar cores das categorias em `category.dart`
- Ajustar paleta de cores no `categories_screen.dart`

## 🐛 Tratamento de Erros

O aplicativo possui tratamento robusto de erros:
- **Validação de Formulários**: Campos obrigatórios e validação de valores
- **Erros de Banco**: Try/catch em todas as operações de banco
- **Feedback Visual**: SnackBars para informar sucessos e erros
- **Estados de Loading**: Indicadores de progresso durante operações

## 📱 Screenshots das Funcionalidades

1. **Tela Principal**: Lista de despesas com total geral
2. **Adicionar Despesa**: Formulário completo com validação
3. **Detalhes**: Visualização completa da despesa
4. **Estatísticas**: Dashboard com gráficos e totais
5. **Categorias**: Gerenciamento com cores e ícones

## 🚨 Observações Importantes

- Os dados são armazenados localmente no dispositivo
- Primeira execução cria automaticamente as categorias padrão
- Exclusão de categorias pode afetar despesas existentes
- Aplicativo funciona offline (não requer internet)

## 🤝 Contribuindo

Para contribuir com o projeto:
1. Faça um fork
2. Crie uma branch para sua feature
3. Commit suas mudanças
4. Push para a branch
5. Abra um Pull Request

## 📄 Licença

Este projeto é open source e está disponível sob a licença MIT.
