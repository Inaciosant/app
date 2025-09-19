# 📱 Como Instalar e Usar o Gerenciador de Despesas

## 🚀 Instalação Rápida

### 1. Pré-requisitos
- Flutter SDK 3.8.1 ou superior
- Android Studio ou VS Code
- Emulador Android ou dispositivo físico

### 2. Comandos de Instalação
```bash
# 1. Navegar até a pasta do projeto
cd "c:\Users\inaci\Documents\Projetos\Mobile\app"

# 2. Instalar dependências
flutter pub get

# 3. Executar o app
flutter run
```

## 📋 Primeiro Uso

### 🎯 Ao Abrir o App Pela Primeira Vez
1. **Tela Principal**: Você verá "Nenhuma despesa registrada"
2. **Categorias**: 8 categorias padrão são criadas automaticamente
3. **Botão +**: Toque para adicionar sua primeira despesa

### ➕ Adicionando Sua Primeira Despesa
1. Toque no botão **"+"** (verde, canto inferior direito)
2. Preencha os campos:
   - **Título**: Ex: "Almoço no restaurante"
   - **Valor**: Ex: "25,50"
   - **Categoria**: Selecione uma das opções (ex: "Alimentação 🍕")
   - **Data**: Padrão é hoje, mas pode alterar
   - **Descrição**: Opcional, ex: "Almoço de trabalho"
3. Toque em **"Salvar Despesa"**
4. Pronto! Sua despesa aparecerá na lista principal

## 🧭 Navegação no App

### 🏠 Tela Principal
- **Lista de Despesas**: Todas as suas despesas, mais recentes primeiro
- **Card de Total**: Mostra o valor total de todos os gastos
- **Botões do Menu**:
  - 📊 **Estatísticas**: Dashboard com gráficos e totais
  - 📁 **Categorias**: Gerenciar suas categorias
- **Ações nas Despesas**:
  - **Toque simples**: Ver detalhes da despesa
  - **Toque longo**: Excluir rapidamente
- **Pull to Refresh**: Puxe a lista para baixo para atualizar

### 📊 Tela de Estatísticas
- **Total Geral**: Soma de todas as suas despesas
- **Este Mês**: Gastos apenas do mês atual
- **Por Categoria**: Veja quanto gasta em cada categoria
- **Despesas Recentes**: Suas últimas 5 transações
- **Estatísticas Extras**: Média diária, categoria mais usada, etc.

### 📁 Gerenciar Categorias
- **Ver Todas**: Lista de categorias existentes
- **Adicionar Nova**: Botão "+" para criar categoria personalizada
- **Editar**: Toque no ícone ✏️ para modificar
- **Excluir**: Toque no ícone 🗑️ para remover (com confirmação)

## 💡 Dicas de Uso

### 🎯 Organizando Suas Despesas
1. **Seja Consistente**: Use sempre as mesmas categorias
2. **Descrições Úteis**: Adicione detalhes que te ajudem a lembrar
3. **Registre Rapidamente**: Anote os gastos assim que acontecem
4. **Revise Regularmente**: Veja as estatísticas semanalmente

### 🎨 Personalizando Categorias
1. **Emojis**: Use emojis relacionados (🍕 para comida, ⛽ para combustível)
2. **Cores**: Escolha cores que façam sentido para você
3. **Nomes Claros**: Use nomes específicos (ex: "Supermercado" ao invés de "Compras")

### 📈 Analisando Gastos
1. **Dashboard**: Visite a tela de estatísticas regularmente
2. **Por Categoria**: Identifique onde gasta mais
3. **Tendências**: Compare gastos mensais para ver padrões
4. **Metas**: Use os dados para estabelecer orçamentos

## 🔧 Solucionando Problemas

### ❌ App não abre
- Verifique se o emulador está funcionando
- Tente: `flutter clean` e depois `flutter run`

### ❌ Erro de localização
- O app foi corrigido para funcionar sem localização pt_BR
- Se persistir, reinicie o app

### ❌ Dados não salvam
- Certifique-se de preencher todos os campos obrigatórios
- Verifique se o valor é um número válido

### ❌ Performance lenta
- Reinicie o app
- Feche outros aplicativos no emulador

## 📱 Funcionalidades Principais

### ✅ O Que Você Pode Fazer
- ➕ **Adicionar despesas** com título, valor, categoria e data
- 👁️ **Ver todas as despesas** em uma lista organizada
- ✏️ **Editar despesas** existentes (toque na despesa → botão editar)
- 🗑️ **Excluir despesas** (toque longo ou botão excluir nos detalhes)
- 📊 **Ver estatísticas** detalhadas dos seus gastos
- 🎨 **Criar categorias** personalizadas com emoji e cor
- 💾 **Dados salvos** automaticamente (funciona offline)

### 🔒 Segurança dos Dados
- **Armazenamento Local**: Seus dados ficam apenas no seu dispositivo
- **Backup Manual**: Anote ou fotografe dados importantes
- **Privacidade**: Nenhum dado é enviado para a internet

## 🎯 Casos de Uso Comuns

### 💳 Controle Diário
1. Anote cada compra no momento que fizer
2. Use categorias específicas
3. Veja o total diário na tela principal

### 📈 Análise Mensal
1. No fim do mês, vá para "Estatísticas"
2. Veja o total mensal e por categoria
3. Compare com meses anteriores

### 🎯 Orçamento por Categoria
1. Anote quanto quer gastar em cada categoria
2. Compare com os valores na tela de estatísticas
3. Ajuste seus hábitos conforme necessário

## 🆘 Suporte

Se encontrar algum problema:
1. Tente reiniciar o app
2. Verifique se todos os campos estão preenchidos
3. Confirme que o emulador está funcionando
4. Execute `flutter doctor` para verificar problemas de configuração

---

**💚 Aproveite seu novo Gerenciador de Despesas!**  
*Desenvolvido para ajudar você a ter controle total sobre suas finanças pessoais.*
