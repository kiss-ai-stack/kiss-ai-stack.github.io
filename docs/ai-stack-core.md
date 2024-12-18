---
layout: document
title: Core / AI Agent Bootstrapper
---

This section is all about the engine of the stack—the part that makes your AI agents come to life with just a YAML configuration file.
<br><br>

### Core Workflow
The diagram below shows how the Core workflow operates step-by-step to process user queries and documents:
<br><br>
![core workflow](/assets/images/core-workflow.png){:style="display:block; margin-left:auto; margin-right:auto; width: 90%"} <br><br>

Here’s what happens under the hood:

##### Step 1: Init
We kick things off and load your YAML config.

##### Step 2: Read YAML Config
We parse your configuration file. Everything starts here.

##### Step 3: Bootstrap Tools
We load and prepare all the tools you’ve configured.

##### Step 4: Create/Retrieve Collection
If you’re working with a Vector DB(RAG tools), we’ll either create a new collection or fetch an existing one.

##### Step 5: Store Documents
Your documents are processed, chunked, and stored in the vector database in appropriate collection. Each RAG tool has a seperate collection.

##### Step 6: Classify and Select Tool
Incoming documents are analyzed, and the right tool is selected for storing the partitioned data chunks.

##### Step 7: User Queries
The AI agent interacts with your users, pulling insights from the tools and the database.

##### Step 8: Classify and Select Tool
Incoming queries are analyzed, and the right tool is selected for the job. If it is a RAG tool it may fetch stored documents next. Otherwise selected prompt tool will generate the answer directly.

##### Step 9: Retrieve Documents
Relevant chunks from the database are fetched when needed(RAG tools).

<br><br>

#### What Makes the Core Special?
The Core is built to keep things easy, fast, and efficient. Here’s why you’ll love it:

- **Zero boilerplate**: Say goodbye to repetitive setup code. Just define your agent in a simple YAML file.  
- **Dynamic tooling**: Automatically loads and configures tools like RAG for document retrieval or prompt-based models for queries.  
- **Autonomous AI powered decision-making**: The built-in decision maker dynamically selects the best tool for each query, optimizing performance.  
- **Vector database adaptors**: Easily store and retrieve private document embeddings with Chroma.  
- **AI/LLM client adaptors**: Seamlessly connect to providers like OpenAI with minimal setup.  
- **Automatic dependency resolver**: Based on the tools dependencies will be resolved automatically in the first run.
- **Temporary/presistant RAG agents**: Cleanup stored documents after temporary sessions.
- **Standalone component**: Core is totally isolated from other components. You may use it standalone in your apps/server developments.

KISS AI Stack Core focuses on keeping it simple while delivering powerful AI capabilities with minimal effort.
<br><br>
