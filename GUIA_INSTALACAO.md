# 🔧 Guia de Instalação e Solução de Problemas

## Gerenciador de Despesas Pessoais - Flutter

### 📋 Pré-requisitos

1. **Flutter SDK** (versão 3.8.1 ou superior)
2. **Android Studio** ou **VS Code**
3. **Android SDK** configurado
4. **Emulador Android** ou **Dispositivo físico**

### 🚀 Instalação

#### 1. Clone/Download do Projeto
```bash
# Se estiver em um repositório Git
git clone [url-do-repositorio]
cd expense_manager

# Ou simplesmente use o projeto atual
```

#### 2. Instalar Dependências
```bash
flutter pub get
```

#### 3. Verificar Ambiente
```bash
flutter doctor
```

#### 4. Executar o App
```bash
flutter run
```

### 🐛 Soluções para Problemas Comuns

#### ❌ Problema: NDK Error
**Erro:** `NDK at [path] did not have a source.properties file`

**Solução:**
1. Abra `android/app/build.gradle.kts`
2. Remova ou comente a linha: `ndkVersion = flutter.ndkVersion`
3. Execute: `flutter clean && flutter pub get`

#### ❌ Problema: Android x86 Warning
**Aviso:** `Support for Android x86 targets will be removed`

**Solução:** Este é apenas um aviso. Para evitá-lo:
1. Use um emulador ARM64 em vez de x86
2. Ou ignore o aviso - não afeta a funcionalidade

#### ❌ Problema: Gradle Build Failed
**Erro:** `FAILURE: Build failed with an exception`

**Soluções:**
```bash
# 1. Limpe o projeto
flutter clean

# 2. Delete pasta build (se existir)
rm -rf build/  # Linux/Mac
rmdir /s build  # Windows

# 3. Reinstale dependências
flutter pub get

# 4. Tente novamente
flutter run
```

#### ❌ Problema: SDK não encontrado
**Erro:** `Android SDK not found`

**Solução:**
1. Abra Android Studio
2. Vá em `Tools > SDK Manager`
3. Instale a versão mais recente do Android SDK
4. Configure as variáveis de ambiente:
   - `ANDROID_HOME` = caminho para o SDK
   - `PATH` += `$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools`

#### ❌ Problema: Emulador não funciona
**Erro:** `No devices found`

**Soluções:**
```bash
# Listar emuladores disponíveis
flutter emulators

# Criar novo emulador (se necessário)
flutter emulators --create

# Iniciar emulador específico
flutter emulators --launch [nome-do-emulador]
```

#### ❌ Problema: Dependências não instaladas
**Erro:** `Package not found` ou imports com erro

**Solução:**
```bash
# Force refresh das dependências
flutter pub deps
flutter pub cache repair
flutter pub get
```

### 📱 Testando o App

#### 1. Emulador Android
```bash
# Listar dispositivos
flutter devices

# Executar em dispositivo específico
flutter run -d [device-id]
```

#### 2. Dispositivo Físico
1. Habilite **Depuração USB** no dispositivo
2. Conecte via USB
3. Aceite a autorização de depuração
4. Execute: `flutter run`

#### 3. Chrome (Web) - Opcional
```bash
flutter run -d chrome
```

### 🔧 Comandos Úteis

```bash
# Verificar ambiente Flutter
flutter doctor -v

# Limpar projeto
flutter clean

# Atualizar dependências
flutter pub upgrade

# Compilar APK de release
flutter build apk --release

# Ver logs detalhados
flutter run --verbose

# Hot reload (durante desenvolvimento)
# Pressione 'r' no terminal ou Ctrl+S no código
```

### 📂 Estrutura de Pastas

```
lib/
├── database/           # SQLite helper
├── models/            # Modelos de dados
├── screens/           # Telas do app
├── utils/             # Utilitários
└── main.dart          # Entrada do app

android/               # Configurações Android
├── app/
│   └── build.gradle.kts
└── build.gradle.kts

pubspec.yaml          # Dependências
```

### ⚙️ Configurações Avançadas

#### Alterar Nome do App
1. **pubspec.yaml**: Mude o campo `name`
2. **android/app/build.gradle.kts**: Altere `applicationId`
3. **android/app/src/main/AndroidManifest.xml**: Modifique `android:label`

#### Alterar Ícone
1. Substitua arquivos em `android/app/src/main/res/mipmap-*/`
2. Ou use: `flutter pub get flutter_launcher_icons`

#### Adicionar Splash Screen
1. Configure em `android/app/src/main/res/drawable/launch_background.xml`
2. Ou use: `flutter pub get flutter_native_splash`

### 🎯 Funcionalidades do App

✅ **Implementadas:**
- CRUD completo de despesas
- Gerenciamento de categorias
- Dashboard com estatísticas
- Banco SQLite local
- Interface responsiva

📝 **Para Expandir:**
- Backup na nuvem
- Gráficos interativos
- Metas de orçamento
- Notificações
- Exportação de relatórios

### 🚨 Troubleshooting Final

Se ainda tiver problemas:

1. **Verifique versões:**
   ```bash
   flutter --version
   dart --version
   ```

2. **Reinstale Flutter** (último recurso):
   - Baixe do site oficial
   - Configure PATH
   - Execute `flutter doctor`

3. **Logs detalhados:**
   ```bash
   flutter run --verbose > log.txt 2>&1
   ```

4. **Comunidade:**
   - [Flutter.dev](https://flutter.dev)
   - [Stack Overflow](https://stackoverflow.com/questions/tagged/flutter)
   - [GitHub Issues](https://github.com/flutter/flutter/issues)

### 📞 Suporte

Para problemas específicos:
1. Verifique este guia primeiro
2. Execute `flutter doctor` e corrija os problemas
3. Tente `flutter clean && flutter pub get`
4. Se persistir, documente o erro completo

---

**💡 Dica:** Mantenha sempre o Flutter atualizado: `flutter upgrade`
