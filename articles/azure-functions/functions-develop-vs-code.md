---
title: Develop Azure Functions by using Visual Studio Code
description: Learn how to develop and test Azure Functions by using the Azure Functions extension for Visual Studio Code.
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: f9ad40cd50863990b9af629c77321195dce5e97c
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2019
ms.locfileid: "74227061"
---
# <a name="develop-azure-functions-by-using-visual-studio-code"></a>Develop Azure Functions by using Visual Studio Code

The [Estensione Funzioni di Azure per Visual Studio Code] lets you locally develop functions and deploy them to Azure. Se questa è la prima esperienza con Funzioni di Azure, è consigliabile leggere altre informazioni in [Introduzione a Funzioni di Azure](functions-overview.md).

The Azure Functions extension provides these benefits:

* Modifica, compilazione ed esecuzione di funzioni nel computer di sviluppo locale.
* Pubblicazione del proprio progetto di Funzioni di Azure direttamente in Azure.
* Write your functions in various languages while taking advantage of the benefits of Visual Studio Code.

The extension can be used with the following languages, which are supported by the Azure Functions version 2.x runtime:

* [C# compiled](functions-dotnet-class-library.md)
* [C# script](functions-reference-csharp.md)<sup>*</sup>
* [JavaScript](functions-reference-node.md)
* [Java](functions-reference-java.md)
* [PowerShell](functions-reference-powershell.md)
* [Python](functions-reference-python.md)

<sup>*</sup>Requires that you [set C# script as your default project language](#c-script-projects).

In this article, examples are currently available only for JavaScript (Node.js) and C# class library functions.  

This article provides details about how to use the Azure Functions extension to develop functions and publish them to Azure. Before you read this article, you should [create your first function by using Visual Studio Code](functions-create-first-function-vs-code.md).

> [!IMPORTANT]
> Don't mix local development and portal development for a single function app. Quando si pubblica da un progetto locale a un'app per le funzioni, il processo di distribuzione sovrascrive le funzioni sviluppate nel portale.

## <a name="prerequisites"></a>Prerequisiti

Before you install and run the [Azure Functions extension][estensione funzioni di azure per visual studio code], you must meet these requirements:

* [Visual Studio Code](https://code.visualstudio.com/) installed on one of the [supported platforms](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* Una sottoscrizione di Azure attiva.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Other resources that you need, like an Azure storage account, are created in your subscription when you [publish by using Visual Studio Code](#publish-to-azure).

> [!IMPORTANT]
> You can develop functions locally and publish them to Azure without having to start and run them locally. To run your functions locally, you'll need to meet some additional requirements, including an automatic download of Azure Functions Core Tools. To learn more, see [Additional requirements for running a project locally](#additional-requirements-for-running-a-project-locally).

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

## <a name="create-an-azure-functions-project"></a>Creare un progetto di Funzioni di Azure

The Functions extension lets you create a function app project, along with your first function. The following steps show how to create an HTTP-triggered function in a new Functions project. [HTTP trigger](functions-bindings-http-webhook.md) is the simplest function trigger template to demonstrate.

1. From **Azure: Functions**, select the **Create Function** icon:

    ![Creare una funzione](./media/functions-develop-vs-code/create-function.png)

1. Select the folder for your function app project, and then **Select a language for your function project**.

1. Select the **HTTP trigger** function template, or you can select **Skip for now** to create a project without a function. You can always [add a function to your project](#add-a-function-to-your-project) later.

    ![Scegliere il modello Trigger HTTP](./media/functions-develop-vs-code/create-function-choose-template.png)

1. Type **HTTPTrigger** for the function name and select Enter, and then select **Function** authorization. This authorization level requires you to provide a [function key](functions-bindings-http-webhook.md#authorization-keys) when you call the function endpoint.

    ![Select Function authorization](./media/functions-develop-vs-code/create-function-auth.png)

    A function is created in your chosen language and in the template for an HTTP-triggered function.

    ![HTTP-triggered function template in Visual Studio Code](./media/functions-develop-vs-code/new-function-full.png)

The project template creates a project in your chosen language and installs required dependencies. For any language, the new project has these files:

* **host.json**: consente di configurare l'host di Funzioni. These settings apply when you're running functions locally and when you're running them in Azure. Per altre informazioni, vedere il [riferimento su host.json](functions-host-json.md).

* **local.settings.json**: Maintains settings used when you're running functions locally. These settings are used only when you're running functions locally. For more information, see [Local settings file](#local-settings-file).

    >[!IMPORTANT]
    >Because the local.settings.json file can contain secrets, you need to exclude it from your project source control.

At this point, you can add input and output bindings to your function by [modifying the function.json file](#add-a-function-to-your-project) or by [adding a parameter to a C# class library function](#add-a-function-to-your-project).

You can also [add a new function to your project](#add-a-function-to-your-project).

## <a name="install-binding-extensions"></a>Installare le estensioni di binding

Except for HTTP and timer triggers, bindings are implemented in extension packages. You must install the extension packages for the triggers and bindings that need them. The process for installing binding extensions depends on your project's language.

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

Run the [dotnet add package](/dotnet/core/tools/dotnet-add-package) command in the Terminal window to install the extension packages that you need in your project. The following command installs the Azure Storage extension, which implements bindings for Blob, Queue, and Table storage.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

---

## <a name="add-a-function-to-your-project"></a>Aggiungere una funzione al progetto

You can add a new function to an existing project by using one of the predefined Functions trigger templates. To add a new function trigger, select F1 to open the command palette, and then search for and run the command **Azure Functions: Create Function**. Follow the prompts to choose your trigger type and define the required attributes of the trigger. If your trigger requires an access key or connection string to connect to a service, get it ready before you create the function trigger.

The results of this action depend on your project's language:

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

A new folder is created in the project. The folder contains a new function.json file and the new JavaScript code file.

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

A new C# class library (.cs) file is added to your project.

---

## <a name="add-input-and-output-bindings"></a>Add input and output bindings

You can expand your function by adding input and output bindings. The process for adding bindings depends on your project's language. To learn more about bindings, see [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).

The following examples connect to a storage queue named `outqueue`, where the connection string for the storage account is set in the `MyStorageConnection` application setting in local.settings.json.

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

Visual Studio Code lets you add bindings to your function.json file by following a convenient set of prompts. To create a binding, right-click (Ctrl+click on macOS) the **function.json** file in your function folder and select **Add binding**:

![Add a binding to an existing JavaScript function ](media/functions-develop-vs-code/function-add-binding.png)

Following are example prompts to define a new storage output binding:

| Prompt | Value | Description |
| -------- | ----- | ----------- |
| **Selezionare la direzione di binding** | `out` | Il binding è un binding di output. |
| **Select binding with direction** | `Azure Queue Storage` | Il binding è un binding della coda di archiviazione di Azure. |
| **Il nome usato per identificare questo binding nel codice** | `msg` | Nome che identifica il parametro di binding a cui viene fatto riferimento nel codice. |
| **La coda a cui verrà inviato il messaggio** | `outqueue` | Il nome della coda in cui scrive il binding. Se *queueName* non esiste, il binding lo crea al primo utilizzo. |
| **Selezionare l'impostazione da "local.setting.json"** | `MyStorageConnection` | The name of an application setting that contains the connection string for the storage account. The `AzureWebJobsStorage` setting contains the connection string for the storage account you created with the function app. |

In this example, the following binding is added to the `bindings` array in your function.json file:

```javascript
{
    "type": "queue",
    "direction": "out",
    "name": "msg",
    "queueName": "outqueue",
    "connection": "MyStorageConnection"
}
```

You can also add the same binding definition directly to your function.json.

In your function code, the `msg` binding is accessed from the `context`, as in this example:

```javascript
context.bindings.msg = "Name passed to the function: " req.query.name;
```

To learn more, see the [Queue storage output binding](functions-bindings-storage-queue.md#output---javascript-example) reference.

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

Update the function method to add the following parameter to the `Run` method definition:

```cs
[Queue("outqueue"),StorageAccount("MyStorageConnection")] ICollector<string> msg
```

This code requires you to add the following `using` statement:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

---

Il parametro `msg` è un tipo `ICollector<T>`, che rappresenta una raccolta di messaggi scritti in un binding di output al completamento della funzione. You add one or more messages to the collection. These messages are sent to the queue when the function completes.

To learn more, see the [Queue storage output binding](functions-bindings-storage-queue.md#output---c-example) documentation.

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="publish-to-azure"></a>Pubblicazione in Azure

Visual Studio Code lets you publish your Functions project directly to Azure. Durante il processo vengono create un'app per le funzioni e le risorse correlate nella sottoscrizione di Azure. L'app per le funzioni fornisce un contesto di esecuzione per le funzioni. Il progetto viene inserito in un pacchetto e distribuito nella nuova app per le funzioni nella sottoscrizione di Azure.

When you publish from Visual Studio Code to a new function app in Azure, you are offered both a quick function app create path and an advanced path. 

When you publish from Visual Studio Code, you take advantage of the [Zip deploy](functions-deployment-technologies.md#zip-deploy) technology. 

### <a name="quick-function-app-create"></a>Quick function app create

When you choose **+ Create new function app in Azure...** , the extension automatically generates values for the Azure resources needed by your function app. These values are based on the function app name that you choose. For an example of using defaults to publish your project to a new function app in Azure, see the [Visual Studio Code quickstart article](functions-create-first-function-vs-code.md#publish-the-project-to-azure).

If you want to provide explicit names for the created resources, you must choose the advanced create path.

### <a name="enable-publishing-with-advanced-create-options"></a>Publish a project to a new function app in Azure by using advanced options

The following steps publish your project to a new function app created with advanced create options:

1. In the **Azure: Functions** area, select the **Deploy to Function App** icon.

    ![Impostazioni dell'app per le funzioni](./media/functions-develop-vs-code/function-app-publish-project.png)

1. If you're not signed in, you're prompted to **Sign in to Azure**. È anche possibile **creare un account di Azure gratuito**. After signing in from the browser, go back to Visual Studio Code.

1. If you have multiple subscriptions, **Select a subscription** for the function app, and then select **+ Create New Function App in Azure... _Advanced_** . This _Advanced_ option gives you more control over the resources you create in Azure. 

1. Following the prompts, provide this information:

    | Prompt | Value | Description |
    | ------ | ----- | ----------- |
    | Select function app in Azure | Create New Function App in Azure | At the next prompt, type a globally unique name that identifies your new function app and then select Enter. I caratteri validi per un nome di app per le funzioni sono `a-z`, `0-9` e `-`. |
    | Select an OS | Windows | The function app runs on Windows. |
    | Select a hosting plan | Piano a consumo | A serverless [Consumption plan hosting](functions-scale.md#consumption-plan) is used. |
    | Select a runtime for your new app | Your project language | The runtime must match the project that you're publishing. |
    | Select a resource group for new resources | Create New Resource Group | At the next prompt, type a resource group name, like `myResourceGroup`, and then select enter. You can also select an existing resource group. |
    | Select a storage account | Creare un nuovo account di archiviazione | At the next prompt, type a globally unique name for the new storage account used by your function app and then select Enter. Storage account names must be between 3 and 24 characters long and can contain only numbers and lowercase letters. You can also select an existing account. |
    | Select a location for new resources | region | Select a location in a [region](https://azure.microsoft.com/regions/) near you or near other services that your functions access. |

    A notification appears after your function app is created and the deployment package is applied. Selezionare **Visualizza output** nelle notifiche per visualizzare i risultati della creazione e della distribuzione, incluse le risorse di Azure create.

## <a name="republish-project-files"></a>Republish project files

When you set up [continuous deployment](functions-continuous-deployment.md), your function app in Azure is updated whenever source files are updated in the connected source location. We recommend continuous deployment, but you can also republish your project file updates from Visual Studio Code.

> [!IMPORTANT]
> La pubblicazione in un'app per le funzioni esistente sovrascrive il contenuto di tale app in Azure.

1. In Visual Studio Code, select F1 to open the command palette. In the command palette, search for and select **Azure Functions: Deploy to function app**.

1. If you're not signed in, you're prompted to **Sign in to Azure**. After you sign in from the browser, go back to Visual Studio Code. If you have multiple subscriptions, **Select a subscription** that contains your function app.

1. Select your existing function app in Azure. When you're warned about overwriting all files in the function app, select **Deploy** to acknowledge the warning and continue.

The project is rebuilt, repackaged, and uploaded to Azure. The existing project is replaced by the new package, and the function app restarts.

## <a name="get-the-url-of-the-deployed-function"></a>Get the URL of the deployed function

To call an HTTP-triggered function, you need the URL of the function when it's deployed to your function app. This URL includes any required [function keys](functions-bindings-http-webhook.md#authorization-keys). You can use the extension to get these URLs for your deployed functions.

1. Select F1 to open the command palette, and then search for and run the command **Azure Functions: Copy Function URL**.

1. Follow the prompts to select your function app in Azure and then the specific HTTP trigger that you want to invoke.

The function URL is copied to the clipboard, along with any required keys passed by the `code` query parameter. Use an HTTP tool to submit POST requests, or a browser for GET requests to the remote function.  

## <a name="run-functions-locally"></a>Eseguire funzioni localmente

The Azure Functions extension lets you run a Functions project on your local development computer. The local runtime is the same runtime that hosts your function app in Azure. Local settings are read from the [local.settings.json file](#local-settings-file).

### <a name="additional-requirements-for-running-a-project-locally"></a>Additional requirements for running a project locally

To run your Functions project locally, you must meet these additional requirements:

* Install version 2.x of [Azure Functions Core Tools](functions-run-local.md#v2). The Core Tools package is downloaded and installed automatically when you start the project locally. Core Tools includes the entire Azure Functions runtime, so download and installation might take some time.

* Installare i requisiti specifici per il linguaggio scelto:

    | Linguaggio | Requisito |
    | -------- | --------- |
    | **C#** | [Estensione C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)<br/>[.NET Core CLI tools](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)   |
    | **Java** | [Debugger for Java extension](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)<br/>[Java 8](https://aka.ms/azure-jdks)<br/>[Maven 3 or later](https://maven.apache.org/) |
    | **JavaScript** | [Node.js](https://nodejs.org/)<sup>*</sup> |  
    | **Python** | [estensione Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)<br/>[Python 3.6.8](https://www.python.org/downloads/) recommended|

    <sup>*</sup>Versioni di Active LTS e Maintenance LTS (consigliate 8.11.1 e 10.14.1).

### <a name="configure-the-project-to-run-locally"></a>Configure the project to run locally

The Functions runtime uses an Azure Storage account internally for all trigger types other than HTTP and webhooks. So you need to set the **Values.AzureWebJobsStorage** key to a valid Azure Storage account connection string.

This section uses the [Azure Storage extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) with [Azure Storage Explorer](https://storageexplorer.com/) to connect to and retrieve the storage connection string.

Per impostare la stringa di connessione dell'account di archiviazione:

1. In Visual Studio, open **Cloud Explorer**, expand **Storage Account** > **Your Storage Account**, and then select **Properties** and copy the **Primary Connection String** value.

2. Nel progetto aprire il file local.settings.json e impostare il valore della chiave **AzureWebJobsStorage** sulla stringa di connessione copiata.

3. Ripetere il passaggio precedente per aggiungere chiavi univoche alla matrice di **Valori** per tutte le altre connessioni richieste dalle funzioni.

For more information, see [Local settings file](#local-settings-file).

### <a name="debugging-functions-locally"></a>Debugging functions locally  

To debug your functions, select F5. If you haven't already downloaded [Core Tools][strumenti di base di funzioni di azure], you're prompted to do so. When Core Tools is installed and running, output is shown in the Terminal. This is the same as running the `func host start` Core Tools command from the Terminal, but with additional build tasks and an attached debugger.  

When the project is running, you can trigger your functions as you would when the project is deployed to Azure. When the project is running in debug mode, breakpoints are hit in Visual Studio Code, as expected.

The request URL for HTTP triggers is displayed in the output in the Terminal. Function keys for HTTP triggers aren't used when a project is running locally. Per altre informazioni, vedere [Strategie per il test del codice in Funzioni di Azure](functions-test-a-function.md).  

To learn more, see [Work with Azure Functions Core Tools][strumenti di base di funzioni di azure].

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

By default, these settings aren't migrated automatically when the project is published to Azure. After publishing finishes, you're given the option of publishing settings from local.settings.json to your function app in Azure. To learn more, see  [Publish application settings](#publish-application-settings).

I valori in **ConnectionStrings** non vengono mai pubblicati.

The function application settings values can also be read in your code as environment variables. For more information, see the Environment variables sections of these language-specific reference articles:

* [C# precompilato](functions-dotnet-class-library.md#environment-variables)
* [Script C# (file con estensione csx)](functions-reference-csharp.md#environment-variables)
* [Java](functions-reference-java.md#environment-variables)
* [JavaScript](functions-reference-node.md#environment-variables)

## <a name="application-settings-in-azure"></a>Application settings in Azure

The settings in the local.settings.json file in your project should be the same as the application settings in the function app in Azure. Any settings you add to local.settings.json must also be added to the function app in Azure. These settings aren't uploaded automatically when you publish the project. Likewise, any settings that you create in your function app [in the portal](functions-how-to-use-azure-function-app-settings.md#settings) must be downloaded to your local project.

### <a name="publish-application-settings"></a>Publish application settings

The easiest way to publish the required settings to your function app in Azure is to use the **Upload settings** link that appears after you publish your project:

![Upload application settings](./media/functions-develop-vs-code/upload-app-settings.png)

You can also publish settings by using the **Azure Functions: Upload Local Setting** command in the command palette. You can add individual settings to application settings in Azure by using the **Azure Functions: Add New Setting** command.

> [!TIP]
> Be sure to save your local.settings.json file before you publish it.

If the local file is encrypted, it's decrypted, published, and encrypted again. If there are settings that have conflicting values in the two locations, you're prompted to choose how to proceed.

View existing app settings in the **Azure: Functions** area by expanding your subscription, your function app, and **Application Settings**.

![View function app settings in Visual Studio Code](./media/functions-develop-vs-code/view-app-settings.png)

### <a name="download-settings-from-azure"></a>Download settings from Azure

If you've created application settings in Azure, you can download them into your local.settings.json file by using the **Azure Functions: Download Remote Settings** command.

As with uploading, if the local file is encrypted, it's decrypted, updated, and encrypted again. If there are settings that have conflicting values in the two locations, you're prompted to choose how to proceed.

## <a name="monitoring-functions"></a>Monitoraggio delle funzioni

When you [run functions locally](#run-functions-locally), log data is streamed to the Terminal console. You can also get log data when your Functions project is running in a function app in Azure. You can either connect to streaming logs in Azure to see near-real-time log data, or you can enable Application Insights for a more complete understanding of how your function app is behaving.

### <a name="streaming-logs"></a>Streaming dei log

When you're developing an application, it's often useful to see logging information in near-real time. You can view a stream of log files being generated by your functions. This output is an example of streaming logs for a request to an HTTP-triggered function:

![Streaming logs output for HTTP trigger](media/functions-develop-vs-code/streaming-logs-vscode-console.png)

To learn more, see [Streaming logs](functions-monitoring.md#streaming-logs).

[!INCLUDE [functions-enable-log-stream-vs-code](../../includes/functions-enable-log-stream-vs-code.md)]

> [!NOTE]
> Streaming logs support only a single instance of the Functions host. When your function is scaled to multiple instances, data from other instances isn't shown in the log stream. [Live Metrics Stream](../azure-monitor/app/live-stream.md) in Application Insights does support multiple instances. While also in near-real time, streaming analytics is based on [sampled data](functions-monitoring.md#configure-sampling).

### <a name="application-insights"></a>Application Insights

We recommend that you monitor the execution of your functions by integrating your function app with Application Insights. When you create a function app in the Azure portal, this integration occurs by default. When you create your function app during Visual Studio publishing, you need to integrate Application Insights yourself.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Per altre informazioni, vedere [Monitorare Funzioni di Azure](functions-monitoring.md).

## <a name="c-script-projects"></a>C\# script projects

By default, all C# projects are created as [C# compiled class library projects](functions-dotnet-class-library.md). If you prefer to work with C# script projects instead, you must select C# script as the default language in the Azure Functions extension settings:

1. Select **File** > **Preferences** > **Settings**.

1. Go to **User Settings** > **Extensions** > **Azure Functions**.

1. Select **C#Script** from **Azure Function: Project Language**.

After you complete these steps, calls made to the underlying Core Tools include the `--csx` option, which generates and publishes C# script (.csx) project files. When you have this default language specified, all projects that you create default to C# script projects. You're not prompted to choose a project language when a default is set. To create projects in other languages, you must change this setting or remove it from the user settings.json file. After you remove this setting, you're again prompted to choose your language when you create a project.

## <a name="command-palette-reference"></a>Command palette reference

The Azure Functions extension provides a useful graphical interface in the area for interacting with your function apps in Azure. The same functionality is also available as commands in the command palette (F1). These Azure Functions commands are available:

|Azure Functions command  | Description  |
|---------|---------|
|**Add New Settings**  |  Creates a new application setting in Azure. To learn more, see [Publish application settings](#publish-application-settings). You might also need to [download this setting to your local settings](#download-settings-from-azure). |
| **Configure Deployment Source** | Connects your function app in Azure to a local Git repository. To learn more, see [Continuous deployment for Azure Functions](functions-continuous-deployment.md). |
| **Connect to GitHub Repository** | Connects your function app to a GitHub repository. |
| **Copy Function URL** | Gets the remote URL of an HTTP-triggered function that's running in Azure. To learn more, see [Get the URL of the deployed function](#get-the-url-of-the-deployed-function). |
| **Create function app in Azure** | Creates a new function app in your subscription in Azure. To learn more, see the section on how to [publish to a new function app in Azure](#publish-to-azure).        |
| **Decrypt Settings** | Decrypts [local settings](#local-settings-file) that have been encrypted by **Azure Functions: Encrypt Settings**.  |
| **Delete Function App** | Removes a function app from your subscription in Azure. When there are no other apps in the App Service plan, you're given the option to delete that too. Other resources, like storage accounts and resource groups, aren't deleted. To remove all resources, you should instead [delete the resource group](functions-add-output-binding-storage-queue-vs-code.md#clean-up-resources). Your local project isn't affected. |
|**Delete Function**  | Removes an existing function from a function app in Azure. Because this deletion doesn't affect your local project, instead consider removing the function locally and then [republishing your project](#republish-project-files). |
| **Delete Proxy** | Removes an Azure Functions proxy from your function app in Azure. To learn more about proxies, see [Work with Azure Functions Proxies](functions-proxies.md). |
| **Delete Setting** | Deletes a function app setting in Azure. This deletion doesn't affect settings in your local.settings.json file. |
| **Disconnect from Repo**  | Removes the [continuous deployment](functions-continuous-deployment.md) connection between a function app in Azure and a source control repository. |
| **Download Remote Settings** | Downloads settings from the chosen function app in Azure into your local.settings.json file. If the local file is encrypted, it's decrypted, updated, and encrypted again. If there are settings that have conflicting values in the two locations, you're prompted to choose how to proceed. Be sure to save changes to your local.settings.json file before you run this command. |
| **Edit settings** | Changes the value of an existing function app setting in Azure. This command doesn't affect settings in your local.settings.json file.  |
| **Encrypt settings** | Encrypts individual items in the `Values` array in the [local settings](#local-settings-file). In this file, `IsEncrypted` is also set to `true`, which specifies that the local runtime will decrypt settings before using them. Encrypt local settings to reduce the risk of leaking valuable information. In Azure, application settings are always stored encrypted. |
| **Execute Function Now** | Manually starts a [timer-triggered function](functions-bindings-timer.md) in Azure. This command is used for testing. To learn more about triggering non-HTTP functions in Azure, see [Manually run a non HTTP-triggered function](functions-manually-run-non-http.md). |
| **Initialize Project for Use with VS Code** | Adds the required Visual Studio Code project files to an existing Functions project. Use this command to work with a project that you created by using Core Tools. |
| **Install or Update Azure Functions Core Tools** | Installs or updates [Strumenti di base di Funzioni di Azure], which is used to run functions locally. |
| **Redeploy**  | Lets you redeploy project files from a connected Git repository to a specific deployment in Azure. To republish local updates from Visual Studio Code, [republish your project](#republish-project-files). |
| **Rename Settings** | Changes the key name of an existing function app setting in Azure. This command doesn't affect settings in your local.settings.json file. After you rename settings in Azure, you should [download those changes to the local project](#download-settings-from-azure). |
| **Riavviare** | Restarts the function app in Azure. Deploying updates also restarts the function app. |
| **Set AzureWebJobsStorage**| Sets the value of the `AzureWebJobsStorage` application setting. This setting is required by Azure Functions. It's set when a function app is created in Azure. |
| **Inizia** | Starts a stopped function app in Azure. |
| **Start Streaming Logs** | Starts the streaming logs for the function app in Azure. Use streaming logs during remote troubleshooting in Azure if you need to see logging information in near-real time. To learn more, see [Streaming logs](#streaming-logs). |
| **Stop** | Stops a function app that's running in Azure. |
| **Stop Streaming Logs** | Stops the streaming logs for the function app in Azure. |
| **Toggle as Slot Setting** | When enabled, ensures that an application setting persists for a given deployment slot. |
| **Uninstall Azure Functions Core Tools** | Removes Azure Functions Core Tools, which is required by the extension. |
| **Upload Local Settings** | Uploads settings from your local.settings.json file to the chosen function app in Azure. If the local file is encrypted, it's decrypted, uploaded, and encrypted again. If there are settings that have conflicting values in the two locations, you're prompted to choose how to proceed. Be sure to save changes to your local.settings.json file before you run this command. |
| **View Commit in GitHub** | Shows you the latest commit in a specific deployment when your function app is connected to a repository. |
| **View Deployment Logs** | Shows you the logs for a specific deployment to the function app in Azure. |

## <a name="next-steps"></a>Passaggi successivi

To learn more about Azure Functions Core Tools, see [Work with Azure Functions Core Tools](functions-run-local.md).

Per altre informazioni sullo sviluppo di funzioni come librerie di classi .NET, vedere [Guida di riferimento per gli sviluppatori C# di Funzioni di Azure](functions-dotnet-class-library.md). This article also provides links to examples of how to use attributes to declare the various types of bindings supported by Azure Functions.

[Estensione Funzioni di Azure per Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
[Strumenti di base di Funzioni di Azure]: functions-run-local.md
