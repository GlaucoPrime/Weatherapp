# 🌦️ WeatherApp

Aplicativo Android nativo para consultar o clima de cidades de interesse, visualizar informações meteorológicas e organizar uma lista de cidades favoritas.

> Projeto acadêmico de TADS — Dispositivos Móveis. O repositório inclui uma análise preliminar de código.

## ✨ Funcionalidades

- Cadastro e login usando Firebase Authentication.
- Consulta do clima atual pela WeatherAPI.
- Consulta de previsão meteorológica.
- Lista de cidades associada ao usuário.
- Visualização de cidades em mapa com Google Maps.
- Persistência remota no Cloud Firestore.
- Cache local da lista de cidades com Room.
- Monitoramento de cidades com tarefas agendadas pelo WorkManager e notificações.

**Observação:** a implementação da notificação agenda e exibe um aviso, mas o `ForecastWorker` analisado não consulta a API para atualizar a previsão antes de notificar. Consulte o relatório de auditoria para limitações e melhorias identificadas.

## 🧰 Tecnologias

- Kotlin
- Android SDK
- Jetpack Compose e Material 3
- MVVM e Repository
- Firebase Authentication
- Cloud Firestore
- Room
- Retrofit e Gson
- WeatherAPI
- Google Maps Compose
- WorkManager
- Kotlin Coroutines e Flow

## 🗂️ Estrutura do projeto

```text
app/
└── src/main/
    ├── AndroidManifest.xml
    └── java/com/weatherapp/
        ├── api/       # Comunicação e modelos da WeatherAPI
        ├── db/
        │   ├── fb/    # Persistência remota no Firestore
        │   └── local/ # Cache local Room
        ├── model/     # Modelos do domínio
        ├── monitor/   # Tarefas agendadas e notificações
        ├── repo/      # Repositório
        ├── ui/        # Componentes e navegação
        ├── HomePage.kt
        ├── ListPage.kt
        ├── LoginActivity.kt
        ├── MainActivity.kt
        ├── MainViewModel.kt
        ├── MapPage.kt
        └── RegisterActivity.kt
docs/
└── auditoria-codigo.md
```

## ⚙️ Pré-requisitos

- Android Studio compatível com a versão do Android Gradle Plugin usada pelo projeto.
- JDK compatível com a configuração Gradle (o projeto declara Java 21).
- SDK Android exigido pelo projeto.
- Projeto Firebase com Authentication por e-mail/senha e Cloud Firestore habilitados.
- Chave da WeatherAPI.
- Chave do Google Maps configurada para Android.

## 🚀 Configuração local

1. Clone o repositório e abra o projeto no Android Studio.
2. Configure o Firebase para o aplicativo Android e adicione o arquivo de configuração exigido pelo Firebase ao local correto do projeto.
3. Crie um arquivo `local.properties` na raiz do projeto (não o envie ao Git) com as propriedades necessárias:

```properties
sdk.dir=CAMINHO_DO_ANDROID_SDK
WEATHER_API_KEY=SUA_CHAVE_WEATHERAPI
MAPS_API_KEY=SUA_CHAVE_GOOGLE_MAPS
```

4. Verifique a configuração do plugin de Secrets do Google Maps e confirme que `MAPS_API_KEY` é disponibilizada para o placeholder usado no `AndroidManifest.xml`.
5. Configure regras de segurança do Firestore para que um usuário autenticado só possa ler e gravar em `users/{uid}` quando `request.auth.uid == uid`, incluindo a subcoleção `cities`. Não use regras abertas em produção.
6. Sincronize o Gradle e execute o app em um emulador ou dispositivo Android.

**Atenção à configuração atual:** `app/build.gradle.kts` lê `WEATHER_API_KEY` diretamente de `local.properties`. Sem esse arquivo, a configuração do build pode falhar. A chave incluída no `BuildConfig` pode ser extraída do APK; restrinja as chaves por aplicação/uso e cotas. `local.properties` estar no `.gitignore` evita o commit acidental, mas não torna uma chave embutida no aplicativo secreta.

Nunca publique chaves reais, tokens, arquivos de credenciais Firebase ou dados pessoais no repositório.

## 🔐 Segurança e privacidade

- A autenticação é delegada ao Firebase Authentication.
- Os campos de senha são mascarados na interface.
- O código organiza as cidades remotas em `users/{uid}/cities`.
- A WeatherAPI usa URL-base HTTPS.
- A lista de cidades tem persistência local com Room.

Esses mecanismos não substituem a revisão das regras do Firestore, a restrição das chaves, os testes de isolamento entre contas e a validação das permissões de localização/notificação. Consulte [`docs/auditoria-codigo.md`](docs/auditoria-codigo.md).

## 🧪 Testes

O projeto inclui dependências de testes Android/JUnit, mas a análise do pacote não encontrou evidência de uma suíte abrangente de testes de negócio e segurança.

Recomenda-se testar:
- Cadastro, login, logout e sessão expirada.
- Acesso entre contas diferentes e regras do Firestore.
- Lista de cidades offline e sincronização após reconexão.
- Permissões concedidas e negadas.
- Falhas HTTP, ausência de rede e respostas vazias da WeatherAPI.
- Quantidade de dias de previsão apresentada.
- Agendamento e cancelamento das notificações.
- Build em ambiente limpo, sem credenciais versionadas.

## 📋 Auditoria de código

O documento [`docs/auditoria-codigo.md`](docs/auditoria-codigo.md) registra o escopo, inventário dos pontos de controle, matriz de risco, análise dos requisitos RNF01–RNF11, achados e plano de ação.

O parecer é **preliminar e parcialmente conforme**: as regras reais do Firestore, as restrições de chaves e o comportamento em execução precisam ser verificados antes de concluir que o sistema está seguro.

## 📌 Status do projeto

Projeto acadêmico em desenvolvimento/avaliação. A presença de uma funcionalidade no código não significa que ela tenha sido validada em testes dinâmicos.

## 👤 Autoria

**Estudante:** [Glauco Vinícius Barbosa Telino]  
**Curso:** Tecnologia em Análise e Desenvolvimento de Sistemas (TADS)  
**Disciplina:** Dispositivos Móveis
