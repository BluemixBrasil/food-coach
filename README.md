# Food Coach sample application [![Build Status](https://travis-ci.org/watson-developer-cloud/food-coach.svg?branch=master)](https://travis-ci.org/watson-developer-cloud/food-coach)

Esta aplicação demonstra como o Conversation pode ser adaptado para usar o Tone Analyzer juntamente com intenções e entidades numa interface de chat simples.
![Demo GIF](readme_images/demo.gif?raw=true)

Demo: http://food-coach.mybluemix.net/

Para mais informações sobre o serviço Conversation, veja [detailed documentation](http://www.ibm.com/watson/developercloud/doc/conversation/overview.shtml).
Para mais informações sobre o Tone Analyzer, veja [detailed documentation](http://www.ibm.com/watson/developercloud/tone-analyzer.html).

# Fazendo Deploy da Aplicação

Se você quiser experimentar o aplicativo ou usá-lo como base para criar seu próprio aplicativo, você precisa implantá-lo em seu próprio ambiente. Você pode explorar os arquivos, fazer alterações e ver como essas alterações afetam o aplicativo em execução. Depois de fazer modificações, você pode implantar a versão modificada do aplicativo no Bluemix.

## Antes de começar

* Você deve ter uma conta no Bluemix, e sua conta deve possuir espaço para 1 aplicação e 2 serviços a serem adicionados. Para criar sua conta no Bluemix, acesse https://console.ng.bluemix.net/registration/. Seu console do Bluemix mostra seu espaço disponível.

* Você deve ter também os seguintes pré-requisitos instalados:
  * O runtime[Node.js](http://nodejs.org/) (incluindo o npm package manager)
  * O [Cloud Foundry command-line client](https://github.com/cloudfoundry/cli#downloads)

## Conseguindo os Arquivos

1. Baixe o código da aplicação Food Coach para o seu computador. Você pode fazer isso das seguintes maneiras:

   * [Baixe o arquivo .zip](https://github.com/watson-developer-cloud/food-coach/archive/master.zip) a partir do repositório Git e extraia os arquivos para um repositório local, OU

   * Use o GitHub para clonar o repositório localmente

## Configurando o serviço Conversation

1. Certifique-se de estar logado no Bluemix usando o Cloud Foundry CLI. Para mais informações, veja [Documentação Watson Developer Cloud](https://www.ibm.com/watson/developercloud/doc/getting_started/gs-cf.shtml).

1. Crie uma instância do serviço Conversation na nuvem IBM:

   ```bash
   cf create-service conversation <service_plan> <service_instance_name>
   ```
   Notes:
      * <service_plan>: opções incluem free, standard ou premium.
      * <service_instance_name>: esse é um nome único de sua escolha.


   Por exemplo:

   ```bash
   cf create-service conversation free conversation-food-coach
   ```

1. Crie uma service key:

   ```bash
   cf create-service-key <service_instance> <service_key>
   ```

   Por exemplo:

   ```bash
   cf create-service-key conversation-food-coach conversation-food-coach-key
   ```

## Configurando o serviço Tone Analyzer

1. Crie uma instância do Tone Analyzer na nuvem IBM:

   ```bash
   cf create-service tone_analyzer <service_plan> <service_instance_name>
   ```
   ```<service_plan>``` opções incluem standard e premium.  Ambas as opções incluem custos.

   Por exemplo:

   ```bash
   cf create-service tone_analyzer standard tone-analyzer-food-coach
   ```

1. Crie uma service key:

   ```bash
   cf create-service-key <service_instance> <service_key>
   ```

   Por exemplo:

   ```bash
   cf create-service-key tone-analyzer-food-coach tone-analyzer-food-coach-key
   ```

### Importando o workspace do Conversation

1. No seu navegador, vá para seu [Bluemix console](https://console.ng.bluemix.net).

1. A partir da aba **Dashboard** , clique no novo serviço Conversation na lista **Services**.  Este terá o nome que você deu no último passo. (e.g., ```<service_instance_name>```).

   ![Screen capture of Services list](readme_images/conversation_food_coach_service.png)

   A pagina de detalhes do serviço se abre.

1. Clique em **Launch tool**.

   ![Screen capture of Launch tool button](readme_images/launch_tool_button.png)

   O Conversation service tool abre.

1. Clique **Import** para adicionar o workspace do foodcoach. Quando solicitado, especifique o local que o arquivo JSON do workspace se encontra na sua cópia local do projeto:

   `<project_root>/food-coach/training/food-coach-workspace.json`

1. Selecione **Everythin (Intents, Entities, and Dialog)** e clique em **Import**. O workspace do food coach é criado.
   * Se você tiver algum problema fazendo o upload do workspace usando o Chrome, por favor tente usando outro browser(Firefox ou Safari).

## Configurando o ambiente da aplicação

1. Na linha de comando, navegue até a pasta local do projeto (`<project_root>/food-coach`).

1. Copie o arquivo `.env.example` em um novo arquvio `.env`. Abra este arquivo num editor de texto.

```bash
   cp .env.example .env
   ```

1. Pegue as credenciais da service key:

   ```bash
   cf service-key <service_instance_name> <service_key>
   ```

   Por exemplo:

   ```bash
   cf service-key conversation-food-coach conversation-food-coach-key
   ```

   A saída deste comando é um objeto JSON, como no exemplo abaixo:

   ```javascript
   {
     "password": "87iT7aqpvU7l",
     "url": "https://gateway.watsonplatform.net/conversation/api",
     "username": "ca2905e6-7b5d-4408-9192-e4d54d83e604"
   }
   ```

1. No JSON de saída, encontre os valores para "password" e "username". Cole estes valores(sem incluir as aspas) nas variáveis `CONVERSATION_PASSWORD` e `CONVERSATION_USERNAME` no arquivo `.env`:

   ```
   CONVERSATION_USERNAME=ca2905e6-7b5d-4408-9192-e4d54d83e604
   CONVERSATION_PASSWORD=87iT7aqpvU7l
   ```
Faça o mesmo para o serviço Tone Analyzer, e cole os valores nas variáveis `TONE_ANALYZER_PASSWORD` e `TONE_ANALYZER_USERNAME` no arquivo `.env`
   ```
   TONE_ANALYZER_USERNAME=mhl715fg-y6h5-2113-6540-ytr78nhs8u64
   TONE_ANALYZER_PASSWORD=124GHaq31M9l
   ```

   Deixe o arquivo `.env` aberto no editor de texto.

1. No seu console do Bluemix, abra a instância do Conversation aonde você importou o workspace.

1. Clique no ícone de menu no canto superior direito do bloco do workspace, e escolha **View details**.

   ![Screen capture of workspace tile menu](readme_images/conversation_food_coach_workspace_details.png)

   O bloco mostra os detalhes do workspace.

1. Clique no ícone ![Copy](readme_images/copy_icon.png) próximo ao workspace ID para copiar o workspace ID.

1. Cole o workspace ID na variável WORKSPACE_ID no arquivo `.env` que você criou anteriormente. Seu arquivo `.env` deve se parecer com o seguinte:

   ![Screen capture of env file](readme_images/env_file_example.png)

Salve e feche o arquivo.

1. Instale os pacotes do Node.js para a aplicação no seu ambiente local:

   ```bash
   npm install
   ```

1. Inicie a aplicação:

    ```bash
    npm start
    ```

A aplicação é instanciada e executada localmente. Vá para `http://localhost:3000` no seu navegador para testá-la.

## Opcional: Instanciândo no Bluemix à partir

Se você quiser fazer o deploy desta aplicação no seu Bluemix, você pode usar o Cloud Foundry.

1. Na pasta raíz do projeto, abra o arquivo `manifest.yml` num editor de texto.

1. Especifíque os seguintes valores no arquivo:

   * Na seção `applications` do arquivo `manifest.yml`, Mude o valor `name` para um nome único para a sua versão da aplicação.
   
   * Na seção `services`, especifíque o nome do serviço Conversation que você criou para a aplicação. Se você não lembrar o nome do serviço, use o comando `cf services` para listar todos os serviços que você criou.

   * Na seção `env`, adicione a variável `WORKSPACE_ID`, especificando o valor do arquivo `.env`.

   O exemplo a seguir mostra uma versão modificada do arquivo `manifest.yml`:   

   ```YAML
   ---
   declared-services:
     conversation-food-coach:
       label: conversation
       plan: free
     tone-analyzer-food-coach:
       label: tone_analyzer
       plan: standard
   applications:
   - name: conversation-food-coach-demo
     command: npm start
     path: .
     memory: 256M
     instances: 1
     services:
     - conversation-food-coach
     - tone-analyzer-food-coach
     env:
       NPM_CONFIG_PRODUCTION: false
       WORKSPACE_ID: fdeab5e4-0ebe-4183-8d10-6e5557a6d842
    ```

1. Salve e feche o arquivo `manifest.yml`.

1. Suba a aplicação para o Bluemix:

   ```bash
   cf push
   ```

Quando o comando terminar de processar, sua aplicação será instanciada e executada no Bluemix. Você pode acessá-la através da URL especificada na saída do comando.

# Próximos passos

Depois que a aplicação estiver instanciada e rodando, experimente como ela responde aos seus inputs.

## Modificando a aplicação

Depois que a aplicação estiver instanciada e rodando, você pode explorar os arquivos fonte e fazer mudanças. Tente o seguinte:

   * Modifique os arquivos .js para fazer mudanças na lógica da aplicação.

   * Modifique o arquivo .html para mudar a aparência da aplicação.

   * Use o Conversation tool para treinar o serviço para novas intenções, entidades, ou tente modificar o fluxo de diálogo. Para mais informações, veja [Conversation service documentation](http://www.ibm.com/watson/developercloud/doc/conversation/index.shtml).

# O que a aplicação Food Coach faz?

A interface da aplicação foi desenhada para conversar com um bot. Baseado na hora do dia, o bot te pergunta se você fez alguma refeição em particular (café da manhã, almoço ou jantar) e o que você comeu nessa refeição.

A interface do chat está no painel da esquerda, e o objeto de resposta JSON, que é retornado pelo Conversation, no painel da direita. Seu input é comparado com um set pequeno de exemplos, treinado com as seguintes intenções:

    yes: entendimento que a refeição especificada foi feita
    no: a refeição especificada não foi feita
    help
    exit

O diálogo também é treinado em 2 tipos de entidades:

    food items
    unhealthy food items

Essas intenções e entidades ajudam o bot a entender variações de input.

Depois de perguntar o que você comeu(se comeu), o bot pergunta como você se sente ao ter comido. Dependendo do seu tom emocional, o bot produz diferentes respostas.

Abaixo você pode ver alguns exemplos de interação:

![Alt text](readme_images/examples.jpeg?raw=true)

Para integrar o Tone Analyzer com o Conversation, a seguinte abordagem foi feita:
   * Interceptar a mensagem do usuário. Antes de mandá-la para o Conversation, invocar o Tone Analyzer. Veja a chamada para `toneDetection.invokeToneAsync` na função `invokeToneConversation` no arquivo [app.js](./app.js).
   * Fazer um parse do JSON de resposta do Tone Analyzer, e adicionar variáveis apropriadas para o objeto context do JSON que será enviado para o Conversation. Veja a função `updateUserTone` no arquivo [tone_detection.js](./addons/tone_detection.js).
   * Mandar o input do usuário, juntamente com o objeto context atualizado no payload para o Conversation. Veja a chamada para `conversation.message` na função `invokeToneConversation` no arquivo [app.js](./app.js).


Você pode ver o JSON de resposta do Conversation no painel da direita.

![Alt text](readme_images/tone_context.jpeg?raw=true)

No template de conversa, respostas alternativas do bot foram fixadas com base no tom emocional do usuário. Por exemplo:

![Alt text](readme_images/rule.png?raw=true)




# Troubleshooting

Se você encontrou algum problema, você pode checar os logs para mais informações. Para ver o log, execute o comando `cf logs`:

   ```bash
   cf logs <application-name> --recent
   ```

# License

  This sample code is licensed under Apache 2.0.
  Full license text is available in [LICENSE](LICENSE).

# Contributing

  See [CONTRIBUTING](CONTRIBUTING.md).

## Open Source @ IBM

  Find more open source projects on the
  [IBM Github Page](http://ibm.github.io/).


