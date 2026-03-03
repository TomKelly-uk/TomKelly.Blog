---
layout: post
title:  "How to get started with Azure AI"
date: 2026-01-29
subtitle: "and how to build your first AI tool"
---
![Entra Dataverse Sync](https://tomkelly.uk/assets/img/Entra%20AD%20User%20Sync/Entra_Dataverse_Link.png)

Before jumping right in, it's worth knowing what is actually possible in Azure AI. So in brief, here is a list of the key capabilities:

- **Azure OpenAI**
  - Provides access to GPT type language models, both large and small, as well as image generation models.
  - Possible use case - Service desk AI for users to ask questions and get answers based on your own company policies.
- **Azure Vision**
  - Provides models for detecting objects, generating captions, or descriptions based on content. 
  - Possible use case - Tracking stock levels.
- **Azure Speech**
  - Models that can perform text to speech or speech to text operations.
  - Possible use case - Transcribing calls with customers for analysis.
- **Azure Language**
  - Can analyse text based natural language and make summarisations. 
  - Possible use case - Categorise incoming tickets.  
- **Microsoft Foundry Content Safety** *- Name change incoming?*
  - Provides algorithms for flagging undesirable content in text and images.
  - Possible use case - Moderating internal chats or LLM output. 
- **Azure Translator**
  - Models that can translate text across a large number of languages
  - Possible use case - Translate call transcripts
- **Azure AI Face**
  - Can detect, analyse and recognise human faces.
  - Possible use case - Secure door access control.  
- **Azure AI Custom Vision**
  - Can be trained for image classification and object detection.
  - Possible use case - Identify laptop models
- **Azure Document Intelligence**
  - Can extract data fields from complex documents
  - Possible use case - Processing expense receipts.
- **Azure Content Understanding**
  - For processing large amounts of documents, images, videos and audio as the same type, extracting their data and organising it into classifications.
  - Possible use case - Generating insights from call centre call recordings or processing applications for completeness. 
- **Azure AI Search**
  - Can create a collection of data that is searchable using natural language. 
  - Possible use case - Internal knowledge base searching or customer support search accuracy improvement. 

## Getting started
All of these tools are accessible through **Microsoft Foundry**, requiring an Azure subscription. Doing it this way allows you to create a project and link multiple AI services together, as opposed to using the standalone resource - giving you more options for future expansion.

## Creating your project
As simple as selecting "Create" ensuring a "Microsoft Foundry Project" is selected. 
![Create Microsoft Foundry Project](assets\img\Azure_AI_Getting_Started\Create_Project.png)

At this point you will need a Azure subscription to assign this against. You didn't think this was free did you? 
  
  It's only as expensive as the model you choose and ofcourse usage. As will all Azure subscriptions, you can also set billing limits so there are no nasty suprises. 

For region, you will want your closest region. However, do watch out - not all models are available in all regions. You may need to research what models are available in your region, or pick the closest. 

## Choose your model
Inside you're project, under "Model Catalog" you will find the vast number of models available for use. 148 at the time of writing this. These can be filtered by task, licenses and you can even see how they compare against eachother in the leaderboards on speed vs quality vs cost etc. 

For this project we are looking to deploy 2 models, one for input/output text based prompting, document and content understanding and another for searching then indexing documents.

For prompting we will be using gpt-5-mini for a good balance of cost and quality. 

  Select your model and click "Use this model", leaving everything default. It is at this point where you may encounter issues if your model is not available in your region - in which a different region will be selected for you. 

We will also be deploying 'text-embedding-ada-002'. This model is used for creating a search index of your data, that way the RAG can retrieve the most relevant data.

## Creating the search index
Before we can use the search index model, we must first create an Azure Search Service resource in Azure. 

1. [Navigate to the Azure the search service](https://portal.azure.com/#create/Microsoft.Search)
2. Select your subscription and resource group
3. Give it a name and location
4. **Select your pricing tier**! This is very important because those costs can balloon. For development purposes I recommend Basic or free. 
5. Review and create

### Connect Azure search to your project
Now that we have a search service, we can link this into your project. 

1. In Microsoft Foundry, navigate to the "Management Center" of your project. 
2. Under project, select "Connected Resources"
3. "New Connection"
4. Select "Azure AI Search"
5. Authentication: Microsoft Entra ID
   1. Best practice tells us that API key should not be used in production. Therefore, follow [these best practices](https://learn.microsoft.com/en-gb/azure/search/search-security-rbac?view=foundry-classic&tabs=roles-portal-admin%2Croles-portal%2Croles-portal-query%2Ctest-portal%2Ccustom-role-portal)  to connect to Azure AI Search using roles. 
6. Add connection

### Installing Microsoft Foundry
We will be using the Visual studio extension, since it simplifies the process of creating projects, deploying and testing AI models. 

After installing and opening [Visual Studio Code](https://code.visualstudio.com/Download), download the Microsoft Foundry extension as shown in the screenshot. Installation can be confirmed with the icon appearing in the left navigation bar. 
![Microsoft Foundry Visual Studio Extension](assets\img\Azure_AI_Getting_Started\Microsoft_Foundry_Extension.png)

### Setting up your environment
Now that we have done the exciting part of choosing our model and creating the project, we now need to actually set up our enviornment. This is done with python through installing a list of packages and setting up our connection to Azure OpenAI. 

Don't worry, its a straightforward couple of steps:
- Download the [latest version of python](https://www.python.org/downloads/)
- Create a project folder with a file called "requirements.txt", containing:
  ```
  azure-ai-projects==1.0.0b10
  azure-ai-inference[prompts]
  azure-identity
  azure-search-documents
  pandas
  python-dotenv
  opentelemetry-api
  ```
  - Inside that folder, run the following on a terminal:
    - ```pip install -r requirements.txt```

### Create your connection string
Now that our packages are installed, time for the connection. This will allow us to call Azure OpenAI from our models. 
- Inside your project folder, create a file called "connection.env" containing the following template:
  ```
  AIPROJECT_CONNECTION_STRING=<your-connection-string>
  AISEARCH_INDEX_NAME="example-index"
  EMBEDDINGS_MODEL="text-embedding-ada-002"
  INTENT_MAPPING_MODEL="gpt-5-mini"
  CHAT_MODEL="gpt-5-mini"
  EVALUATION_MODEL="gpt-5-mini"
  ```
- Replace the ```<your-connection-string>``` section with the Microsoft Foundry project endpoint, that can be found in the overview section of your foundry project. 

### Install Azure CLI
This is needed for us to XXXXXXX

- In your VS Code terminal, run ```winget install -e --id Microsoft.AzureCLI```
- Kill and reboot your terminal and run ```az login```
  - If you are getting errors relating to az not being recognised, its likely your [PATH environment variable hasn't updated](#UpdatePathEnv).
- Login with your account with the Azure subscription


--------------

### Test your model
Put audio into it 
### Management center
  This is where you can configure the settings at a resource level or the  project level. This includes setting access permissions and connect to other resources. 

  A quick clarification of the difference between resources and project, since I don't believe this is clear enough:
   - **Resource level** - This is top level container, a Foundry Resource that provides the connections and access to Azure. Think of this as your environment. 
     - Note the resource group that was created in Azure to support this project. Can be found in the "Overview" section, under "Resource Properties".
   - **Project level** - This is the project that sits inside of your foundry resource. This is where you build your model and data. Think of this as your solution.
     - Note the "Go to project" link to return back to the project overview. 

### Connecting to other applications
Connecting our project into other applications, known as endpoints is done utilising the "Endpoints and keys" section in the Overview page. By using these keys in your application code, you will be able to connect your application to this project and the AI models within. 

---------------

Since we are just dipping our toe into the water of Azure AI, we will be creating a basic chat application that uses [Retrieval Augmented Generation (RAG)]!(https://learn.microsoft.com/en-gb/azure/ai-foundry/concepts/retrieval-augmented-generation?view=foundry-classic). This will allow for responses that are grounded in our own content, as opposed to MadMax42's reddit comment from 4 years ago. 

## Troubleshooting
### Path environment variable hasn't updated correctly {#UpdatePathEnv}
1. Press Win + I
2. Go to System -> About
3. Go to Advanced system settings
4. Click Environment Variables
5. Click New
6. Add your necesarry path, e.g. ```C:\Program Files\Microsoft SDKs\Azure\CLI2\wbin```