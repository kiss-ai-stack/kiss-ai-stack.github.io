---
layout: document
title: Core - Configuration Guide
---



This YAML configuration file defines the structure and functionality of the AI stack, covering decision-making, available tools, and the vector database setup for Retrieval-Augmented Generation (RAG).



## Configuration Overview
The configuration file can be placed in the root directory as `stack.yaml` or specified via an environment variable: `STACK_CONFIG="/path/stack.yaml"`




### stack

The `stack` section specifies the core components, including the decision-maker and tools.



#### decision_maker

This component classifies tools for incoming queries.
- **`name`**: Decision-maker name.
- **`role`**: Describes its purpose, such as classifying tools for queries.
- **`kind`**: Specifies decision-making type:
  - `prompt`: Prompt-based classification.
  - `rag`: Not supported.
- **`ai_client`**: AI client configuration.
  - **`provider`**: AI provider (e.g., `openai`).
  - **`model`**: AI model (e.g., `gpt-4`).
  - **`api_key`**: API key for accessing the service.



#### tools

Defines the tools available to the stack for processing queries.

Each tool has the following properties:
- **`name`**: Tool name.
- **`role`**: Describes its purpose.
- **`kind`**: Tool type:
  - `prompt`: For prompt-based processing.
  - `rag`: For RAG-based document processing.
- **`depth`**: Number of document sets to consider for RAG-based tools.
- **`ai_client`** (if `kind` is `prompt`):
  - **`provider`**: AI provider (e.g., `openai`).
  - **`model`**: AI model (e.g., `gpt-4`).
  - **`api_key`**: API key for the service.
- **`embeddings`** (if `kind` is `rag`): Embedding model for document processing (e.g., `text-embedding-ada-002`).

##### Example Tools:
1. **`general_queries`**: Handles general queries using a prompt-based approach with `gpt-4`.
2. **`document_tool`**: Processes documents and generates answers using a RAG approach with `text-embedding-ada-002` embeddings.



#### vector_db

Configures the vector database for RAG.

##### Properties:
- **`provider`**: Vector database provider (e.g., `chroma`).
- **`kind`**: Storage type:
  - `in-memory`: Temporary storage.
  - `storage`: Persistent local storage.
  - `remote`: Hosted remote database.
- **`host`**: Database host address (e.g., `0.0.0.0`).
- **`port`**: Database port number (e.g., `8000`).
- **`secure`**: Boolean indicating secure connection (`true` or `false`).



## Example Configuration


```yaml
stack:
  decision_maker:
    name: decision_maker
    role: classify tools for queries
    kind: prompt
    ai_client:
      provider: openai
      model: gpt-4
      api_key: <your-api-key>

  tools:
    - name: general_queries
      role: Handle other queries if no suitable tool is found.
      kind: prompt
      ai_client:
        provider: openai
        model: gpt-4
        api_key: <your-api-key>

    - name: document_tool
      role: Process documents and provide answers.
      kind: rag
      embeddings: text-embedding-ada-002
      ai_client:
        provider: openai
        model: gpt-4
        api_key: <your-api-key>
      depth: 4

  vector_db:
    provider: chroma
    kind: remote
    host: 0.0.0.0
    port: 8000
    secure: false
```

<br><br>

___
