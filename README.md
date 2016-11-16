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

   The Service Details page opens.

1. Click the **Launch tool** button.

   ![Screen capture of Launch tool button](readme_images/launch_tool_button.png)

   The Conversation service tool opens.

1. Click **Import** to add the food coach workspace. When prompted, specify the location of the workspace JSON file in your local copy of the application project:

   `<project_root>/food-coach/training/food-coach-workspace.json`

1. Select **Everything (Intents, Entities, and Dialog)** and then click **Import**. The food coach workspace is created.
   * If you have any problems uploading the workspace using Chrome, please try another browser such as Firefox or Safari.

## Configuring the application environment

1. At the command line, navigate to the local project directory (`<project_root>/food-coach`).

1. Copy the `.env.example` file to a new `.env` file. Open this file in a text editor.

```bash
   cp .env.example .env
   ```

1. Retrieve the credentials from the service key:

   ```bash
   cf service-key <service_instance_name> <service_key>
   ```

   For example:

   ```bash
   cf service-key conversation-food-coach conversation-food-coach-key
   ```

   The output from this command is a JSON object, as in this example:

   ```javascript
   {
     "password": "87iT7aqpvU7l",
     "url": "https://gateway.watsonplatform.net/conversation/api",
     "username": "ca2905e6-7b5d-4408-9192-e4d54d83e604"
   }
   ```

1. In the JSON output, find the values for the `password` and `username` keys. Paste these values (not including the quotation marks) into the `CONVERSATION_PASSWORD` and `CONVERSATION_USERNAME` variables in the `.env` file:

   ```
   CONVERSATION_USERNAME=ca2905e6-7b5d-4408-9192-e4d54d83e604
   CONVERSATION_PASSWORD=87iT7aqpvU7l
   ```
Do the same for the Tone Analyzer service, and paste the values into the `TONE_ANALYZER_PASSWORD` and `TONE_ANALYZER_USERNAME` variables in the `.env` file
   ```
   TONE_ANALYZER_USERNAME=mhl715fg-y6h5-2113-6540-ytr78nhs8u64
   TONE_ANALYZER_PASSWORD=124GHaq31M9l
   ```

   Leave the `.env` file open in your text editor.

1. In your Bluemix console, open the Conversation service instance where you imported the workspace.

1. Click the menu icon in the upper right corner of the workspace tile, and then select **View details**.

   ![Screen capture of workspace tile menu](readme_images/conversation_food_coach_workspace_details.png)

   The tile shows the workspace details.

1. Click the ![Copy](readme_images/copy_icon.png) icon next to the workspace ID to copy the workspace ID to the clipboard.

1. Back on your local system, paste the workspace ID into the WORKSPACE_ID variable in the `.env` file you previously created. At this point, your `.env` file should look like the following:

   ![Screen capture of env file](readme_images/env_file_example.png)

Save and close the file.

1. Install the demo application package into the local Node.js runtime environment:

   ```bash
   npm install
   ```

1. Start the application:

    ```bash
    npm start
    ```

The application is now deployed and running on the local system. Go to `http://localhost:3000` in your browser to try it out.

## Optional: Deploying from the local system to Bluemix

If you want to subsequently deploy your local version of the application to the Bluemix cloud, you can use Cloud Foundry.

1. In the project root directory, open the `manifest.yml` file in a text editor.

1. Specify the following values in the file:

   * In the `applications` section of the `manifest.yml` file, change the `name` value to a unique name for your version of the demo application.

   * In the `services` section, specify the name of the Conversation service instance you created for the demo application. If you do not remember the service name, use the `cf services` command to list all services you have created.

   * In the `env` section, add the `WORKSPACE_ID` environment variable, specifying the value from the `.env` file.

   The following example shows a modified `manifest.yml` file:   

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

1. Save and close the `manifest.yml` file.

1. Push the application to Bluemix:

   ```bash
   cf push
   ```

When the command finishes processing, your application is deployed and running on Bluemix. You can access it using the URL specified in the command output.

# What to do next

After you have the application installed and running, experiment with it to see how it responds to your input.

## Modifying the application

After you have the application deployed and running, you can explore the source files and make changes. Try the following:

   * Modify the .js files to change the application logic.

   * Modify the .html file to change the appearance of the application page.

   * Use the Conversation tool to train the service for new intents, or to modify the dialog flow. For more information, see the [Conversation service documentation](http://www.ibm.com/watson/developercloud/doc/conversation/index.shtml).

# What does the Food Coach application do?

The application interface is designed for chatting with a coaching bot. Based on the time of day, it asks you if you've had a particular meal (breakfast, lunch, or dinner) and what you ate for that meal.

The chat interface is in the left panel of the UI, and the JSON response object returned by the Conversation Service in the right panel. Your input is run against a small set of sample data trained with the following intents:

    yes: acknowledgment that the specified meal was eaten
    no: the specified meal was not eaten
    help
    exit

The dialog is also trained on two types of entities:

    food items
    unhealthy food items

These intents and entities help the bot understand variations your input.

After asking you what you ate (if a meal was consumed), the bot asks you how you feel about it. Depending on your emotional tone, the bot provides different feedback.

Below you can find some sample interactions:

![Alt text](readme_images/examples.jpeg?raw=true)

In order to integrate the Tone Analyzer with the Conversation service, the following approach was taken:
   * Intercept the user's message. Before sending it to the Conversation Service, invoke the Tone Analyzer Service. See the call to `toneDetection.invokeToneAsync` in the `invokeToneConversation` function in [app.js](./app.js).
   * Parse the JSON response object from the Tone Analyzer Service, and add appropriate variables to the context object of the JSON payload to be sent to the Conversation Service. See the `updateUserTone` function in [tone_detection.js](./addons/tone_detection.js).
   * Send the user input, along with the updated context object in the payload to the Conversation Service. See the call to `conversation.message` in the `invokeToneConversation` function in [app.js](./app.js).


You can see the JSON response object from the Conversation Service in the right hand panel.

![Alt text](readme_images/tone_context.jpeg?raw=true)

In the conversation template, alternative bot responses were encoded based on the user's emotional tone. For example:

![Alt text](readme_images/rule.png?raw=true)




# Troubleshooting

If you encounter a problem, you can check the logs for more information. To see the logs, run the `cf logs` command:

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


