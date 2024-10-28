# Vector Store RAG Demo

This sample demonstrates how to **ingest** text from **PDF** files into a **vector store**

Also ask questions about the content using an **LLM(Large Language Model)** while using **RAG(Retrieval Augment Generate)** to supplement the LLM with additional information from the vector store

![image](https://github.com/user-attachments/assets/0b17e275-2d3d-4a52-a287-5781d7542c85)

## 1. Configuring the Sample

The sample can be configured in various ways:

### 1.1 Choose the Vector Store

Choose your preferred vector store by setting the **Rag:VectorStoreType** configuration setting in the **appsettings.json** file to one of the following values:
   
AzureAISearch
   
AzureCosmosDBMongoDB
   
AzureCosmosDBNoSQL
   
InMemory

Qdrant
   
Redis
   
Weaviate

### 1.2. Choose the AI Chat Service

You can choose your preferred AI Chat service by settings the **Rag:AIChatService** configuration setting in the **appsettings.json** file to one of the following values:
  
AzureOpenAI
   
OpenAI

### 1.3. Choose the AI Embedding Service 

You can choose your preferred AI Embedding service by settings the **Rag:AIEmbeddingService** configuration setting in the **appsettings.json** file to one of the following values:
   
AzureOpenAIEmbeddings
   
OpenAIEmbeddings

### 1.4. Load data into the vector store or Loaded data previously

You can choose whether to load data into the vector store by setting the **Rag:BuildCollection** configuration setting in the **appsettings.json** file to **true**

If you set this to **false**, the sample will assume that data was already loaded previously and it will go straight into the chat experience

### 1.5. Input the CollectionName

You can choose the name of the collection to use by setting the **Rag:CollectionName** configuration setting in the **appsettings.json** file

### 1.6. Choose the PDF File to load

You can choose the pdf file to load into the vector store by setting the **Rag:PdfFilePaths** array in the **appsettings.json** file

### 1.7. Set the number of records to process

You can choose the number of records to process per batch when loading data into the vector store by setting the **Rag:DataLoadingBatchSize** configuration setting in the **appsettings.json** file

### 1.8. Set the time to wait between batches

You can choose the number of milliseconds to wait between batches when loading data into the vector store by setting the **Rag:DataLoadingBetweenBatchDelayInMilliseconds** configuration setting in the **appsettings.json** file

## 2. Dependency Setup

To run this sample, you need to setup your source data, setup your vector store and AI services, and setup secrets for these

### 2.1. Source PDF File

You will need to supply some source **pdf files** to load into the **vector store**

Once you have a file ready, update the **PdfFilePaths** array in the **appsettings.json** file with the path to the file

```json
{
    "Rag": {
        "PdfFilePaths": [ "sourcedocument.pdf" ],
    }
}
```

Why not try the semantic kernel documentation as your input

You can download it as a PDF from the https://learn.microsoft.com/en-us/semantic-kernel/overview/ page

See the Download PDF button at the bottom of the page.

### 2.2. Azure OpenAI Chat Completion

For **Azure OpenAI Chat Completion**, you need to add the following secrets:

```cli
dotnet user-secrets set "AIServices:AzureOpenAI:Endpoint" "https://<yourservice>.openai.azure.com"
dotnet user-secrets set "AIServices:AzureOpenAI:ChatDeploymentName" "<your deployment name>"
```

Note that the code doesn't use an **API Key** to communicate with Azure Open AI, but rather an **AzureCliCredential** so no api key secret is required

### 2.3. OpenAI Chat Completion

For **OpenAI Chat Completion**, you need to add the following secrets:

```cli
dotnet user-secrets set "AIServices:OpenAI:ModelId" "<your model id>"
dotnet user-secrets set "AIServices:OpenAI:ApiKey" "<your api key>"
```

Optionally, you can also provide an **Org Id**

```cli
dotnet user-secrets set "AIServices:OpenAI:OrgId" "<your org id>"
```

### 2.4. Azure OpenAI Embeddings

For **Azure OpenAI Embeddings**, you need to add the following secrets:

```cli
dotnet user-secrets set "AIServices:AzureOpenAIEmbeddings:Endpoint" "https://<yourservice>.openai.azure.com"
dotnet user-secrets set "AIServices:AzureOpenAIEmbeddings:DeploymentName" "<your deployment name>"
```

Note that the code doesn't use an **API Key** to communicate with Azure Open AI, but rather an **AzureCliCredential** so no api key secret is required

### 2.5. OpenAI Embeddings

For **OpenAI Embeddings**, you need to add the following secrets:

```cli
dotnet user-secrets set "AIServices:OpenAIEmbeddings:ModelId" "<your model id>"
dotnet user-secrets set "AIServices:OpenAIEmbeddings:ApiKey" "<your api key>"
```

Optionally, you can also provide an **Org Id**

```cli
dotnet user-secrets set "AIServices:OpenAIEmbeddings:OrgId" "<your org id>"
```

### 2.6. Azure AI Search

If you want to use **Azure AI Search** as your **Vector Store**, you will need to create an instance of Azure AI Search and add the following **secrets** here:

```cli
dotnet user-secrets set "VectorStores:AzureAISearch:Endpoint" "https://<yourservice>.search.windows.net"
dotnet user-secrets set "VectorStores:AzureAISearch:ApiKey" "<yoursecret>"
```

### 2.7. Azure CosmosDB MongoDB

If you want to use **Azure CosmosDB MongoDB** as your **Vector Store**, you will need to create an instance of Azure CosmosDB MongoDB and add the following **secrets** here:

```cli
dotnet user-secrets set "VectorStores:AzureCosmosDBMongoDB:ConnectionString" "<yourconnectionstring>"
dotnet user-secrets set "VectorStores:AzureCosmosDBMongoDB:DatabaseName" "<yourdbname>"
```

### 2.8. Azure CosmosDB NoSQL

If you want to use **Azure CosmosDB NoSQL** as your **Vector Store**, you will need to create an instance of Azure CosmosDB NoSQL and add the following **secrets** here:

```cli
dotnet user-secrets set "VectorStores:AzureCosmosDBNoSQL:ConnectionString" "<yourconnectionstring>"
dotnet user-secrets set "VectorStores:AzureCosmosDBNoSQL:DatabaseName" "<yourdbname>"
```

### 2.9. Qdrant

If you want to use **Qdrant** as your **Vector Store**, you will need to have an instance of Qdrant available

You can use the following command to start a **Qdrant instance in Docker**, and this will work with the default configured settings:

```cli
docker run -d --name qdrant -p 6333:6333 -p 6334:6334 qdrant/qdrant:latest
```

If you want to use a **different instance of Qdrant**, you can update the **appsettings.json** file or add the following secrets to reconfigure:

```cli
dotnet user-secrets set "VectorStores:Qdrant:Host" "<yourservice>"
dotnet user-secrets set "VectorStores:Qdrant:Port" "6334"
dotnet user-secrets set "VectorStores:Qdrant:Https" "true"
dotnet user-secrets set "VectorStores:Qdrant:ApiKey" "<yoursecret>"
```

### 2.10. Redis

If you want to use **Redis** as your **Vector Store**, you will need to have an instance of Redis available

You can use the following command to start a **Redis instance in Docker**, and this will work with the default configured settings:

```cli
docker run -d --name redis-stack -p 6379:6379 -p 8001:8001 redis/redis-stack:latest
```

If you want to use a **different instance of Redis**, you can update the appsettings.json file or add the following secret to reconfigure:

```cli
dotnet user-secrets set "VectorStores:Redis:ConnectionConfiguration" "<yourredisconnectionconfiguration>"
```

### 2.11. Weaviate

If you want to use **Weaviate** as your **Vector Store**, you will need to have an instance of Weaviate available.

You can use the following command to start a **Weaviate instance in Docker**, and this will work with the default configured settings:

```cli
docker run -d --name weaviate -p 8080:8080 -p 50051:50051 cr.weaviate.io/semitechnologies/weaviate:1.26.4
```

If you want to use a **different instance of Weaviate**, you can update the **appsettings.json** file or add the following secret to reconfigure:

```cli
dotnet user-secrets set "VectorStores:Weaviate:Endpoint" "<yourweaviateurl>"
```
