## Neon & .NET Semantic Kernel Example

This repository provides examples of how to use Neon Serverless Postgres, in combination with .NET console app and the Semantic Kernel. It includes a Retrieval-Augmented Generation (RAG) workflow, demonstrating how to embed and search academic papers using Azure OpenAI embeddings stored in a Neon vector store.

- [.NET SDK](https://dotnet.microsoft.com/en-us/download)
- [Azure account](https://azure.microsoft.com/en-gb/Free) and [Azure OpenAI API](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference) credentials
- A free [Neon account](https://console.neon.tech/signup)
- An [Azure account](https://azure.microsoft.com/free/) with an active subscription

### Setup

To set up the project, you need to install the required dependencies. You can do this by running the following commands in the `dotnet` directory:

```sh
cd dotnet
copy appsettings.example.json appsettings.json
# Update appsettings.json with your Neon Database and Azure OpenAI settings

dotnet restore
dotnet build
```

### Configuration

Before running the examples, you need to configure your database and Azure OpenAI settings. Use the `appsettings.example.json` file in the `dotnet` directory as a template to create an `appsettings.json` file with the proper settings for your database. Update the connection details for your Neon database and Azure OpenAI instance in the `appsettings.json` file.

### Using Neon Serverless Postgres

Neon provides a fully managed PostgreSQL database with automatic scaling and branching, making it an excellent choice for AI-driven applications that require high availability and performance.

#### Set Up Neon Project

1. Navigate to the [Neon Console](https://console.neon.tech/)
2. Click "New Project"
3. Select **Azure** as your cloud provider
4. Choose East US 2 as your region
5. Give your project a name (e.g., "generative-feedback-loop")
6. Click "Create Project"
7. Once the project is created successfully, copy the Neon **connection string.** You can find the connection details in the **Connection Details** widget on the Neon **Dashboard.**

### Vector Store Retrieval Augmented Generation (RAG)

The `Vector Store RAG` example demonstrates how to search the [arXiv API](http://export.arxiv.org/) for specific papers based on a topic and category, embed the abstracts using OpenAI's embedding service, and store these embeddings in a [Neon vector store](https://neon.tech/docs/extensions/pgvector) for efficient searching. It includes commands for loading data, searching data, and querying the model.

#### Running the Example

To run the example, navigate to the `dotnet` directory and execute the following commands:

##### Load Data

```sh
dotnet run --project PGSKExamples.csproj load --topic RAG --total 100
```

This will load the data from arXiv papers based on the specified topic and category. The `category` parameter should be one of the arXiv categories, which you can find [here](https://arxiv.org/category_taxonomy). The abstracts of the papers are then embedded using Azure OpenAI's embedding service, and these embeddings are stored in a Neon vector store for efficient searching.

##### Query Data

```sh
dotnet run --project PGSKExamples.csproj query "Your query here"
```

This will search the loaded arXiv papers based on the provided query by comparing the query embedding against the stored embeddings in the PostgreSQL vector store.

Example output, loaded ~600 RAG-related papers:

```sh
> dotnet run query "What are good chunking strategies to use for unstructured text in RAG applications?"
```

Output:

```markdown
Here's a list of strategies and insights about chunking for unstructured text, derived from recent research articles:

### Recommended Chunking Strategies for RAG Applications

1. **Recursive Character Splitting** -
   The Recursive Character Splitter is shown to outperform Token-based Splitters in preserving contextual integrity during document splitting. This method is particularly effective for maintaining the coherence and capturing the meaning essential for retrieval tasks. [Paper: [Exploring Information Retrieval Landscapes](http://arxiv.org/abs/2409.08479v2)]

2. **Node-based Extraction** -
   For documents with highly diverse structures, employing node-based extraction with LLM-powered Optical Character Recognition (OCR) improves chunking by creating context-aware relationships between text components (e.g., headers and sections). This is crucial for multimodal documents like presentations and scanned files. [Paper: [Advanced ingestion process powered by LLM parsing](http://arxiv.org/abs/2412.15262v1)]

3. **Inference-time Hybrid Structuring** -
   Structured reconstruction of documents is advocated to handle knowledge-intensive tasks better. This involves optimizing the document format for task-specific structuring using "StructRAG" frameworks, which determine the optimal chunk size and type for retrieving relevant information accurately. [Paper: [StructRAG: Boosting Knowledge Intensive Reasoning](http://arxiv.org/abs/2410.08815v2)]

4. **Interpretable Knowledge Segmentation** -
   Chunking HTML or unstructured text into meaningful units for specific downstream tasks can be enhanced by pre-trained models combined with algorithms for efficient segmentation. This improves HTML or table data understanding and retrieval from unstructured text. [Paper: [Leveraging Large Language Models for Web Scraping](http://arxiv.org/abs/2406.08246v1)]

5. **Benchmark-Driven Optimization** -
   Use domain-specific benchmarks (e.g., UDA Suite) to evaluate and optimize chunking and retrieval methods. Real-world applications, involving lengthy or noisy documents in diverse formats, benefit from such approaches to balance character and word-level chunking boundaries. [Paper: [UDA: A Benchmark Suite for RAG](http://arxiv.org/abs/2406.15187v2)]

### Practical Suggestions:

- **Context Preservation with Overlap**: Introducing a degree of overlap between adjacent chunks ensures preserved meaning and continuity.

- **Multimodal Parsing**: Different parsing strategies for highly unstructured data types improve the granularity of retrieval strategies (i.e., PDFs, presentations).

- **Evaluation Metrics**: Incorporating advanced evaluation techniques like SequenceMatcher, BLEU, METEOR, and BERT Score can guide chunking methodology tailoring for retrieval accuracy.

Would you like a deep dive into any of the specific papers or methods listed above?

### Additional Information

For more details on the project and its usage, refer to the comments and documentation within the codebase.