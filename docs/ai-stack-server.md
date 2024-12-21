---
layout: document
title: KISS AI Stack - Server Stub
---

The KISS AI Stack Server is a server stub designed to support RESTful and WebSocket APIs for handling AI-stack sessions with `kiss-ai-stack-core` tasks like stack lifecycle management, query execution, and document storage.


## Features

- **REST API**: Supports authentication, session actions, queries, and document storage.
- **WebSocket API**: Enables real-time, event-driven interactions.
- **Session Management**: Handles persistent and temporary sessions.
- **Lifecycle Events**: Flexible architecture to handle server events during the AI stack’s lifecycle.

## Stack Session Lifecycle Events

- **`on_auth`**: Authenticate a session.
- **`on_init`**: Initialize a session.
- **`on_close`**: Close a session.
- **`on_query`**: Execute a query.
- **`on_store`**: Store documents.

## Getting Started

### Installation

1. **Install the KISS AI Stack Server package**:

```bash
pip install kiss-ai-stack-server~=0.1.0-alpha20
```

2. **Set environment variables**:

```shell
ACCESS_TOKEN_SECRET_KEY = "your-secure-random-secret-key"
ACCESS_TOKEN_ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

SESSION_DB_URL="sqlite://sessions.db"

LOG_LEVEL=INFO
```

3. **Add stack bootstrapping YAML configuration**:

```yaml
stack:
  decision_maker:
    name: decision_maker
    role: classify tools for given queries
    kind: prompt
    ai_client:
      provider: openai
      model: gpt-4
      api_key: <your-api-key>

  tools:
    - name: general_queries
      role: process other queries if no suitable tool is found.
      kind: prompt
      ai_client:
        provider: openai
        model: gpt-4
        api_key: <your-api-key>

    - name: document_tool
      role: process documents and provide answers based on them.
      kind: rag
      embeddings: text-embedding-ada-002
      ai_client:
        provider: openai
        model: gpt-4
        api_key: <your-api-key>

  vector_db:
    provider: chroma
    kind: remote
    host: 0.0.0.0
    port: 8000
    secure: false
```

4. **Start the server**:

```python
import asyncio
import uvicorn

from kiss_ai_stack_server import bootstrap_session_schema, stacks_server, get_stack_server_app

async def start_server():
    await bootstrap_session_schema()
    server_app = get_stack_server_app()
    await stacks_server(
        config=uvicorn.Config(
            app=server_app,
            host='0.0.0.0',
            port=8080
        )
    ).serve()

asyncio.run(start_server())
```

---

## REST API Endpoints

### 1. Authentication

**Endpoint:** `/auth`  
**Method:** `POST`  

**Request Body:**
```json
{
   "scope": "string", // `temporary` or `persistent` to create a new session
   "client_id": "string", // client_id and client_secret from previous session; for `persistent` scope only
   "client_secret": "string"
}
```

**Response:**
```json
{
   "access_token": "string",
   "client_id": "string", // for a `persistent` session, keep the client_id and client_secret saved to refresh the access token
   "client_secret": "secret"
}
```

### 2. Session Actions

**Endpoint:** `/sessions?action={init|close}`  
**Method:** `POST`  
**Query Parameter:**

- `action` (required): Action to perform on the session (`init` or `close`).
  - `init`: Initializes the stack session.
  - `close`: Closes the active session; for `temporary` scope, stored docs will be cleaned.

**Request Body:**

```json
{
  "query": "Optional[string]" // Your greeting message perhaps
}
```

### 3. Query Execution

**Endpoint:** `/queries`  
**Method:** `POST`  
**Request Body:**

```json
{
  "query": "string"
}
```

**Response:** Generated answer.

### 4. Document Storage

**Endpoint:** `/documents`  
**Method:** `POST`  
**Request Body:**

```json
{
   "files": [
      {
         "name": "string", // file name
         "content": "string" // base64 encoded file data
      },
      "metadata": "Dictionary" // Additional metadata
   ]
}
```
**Response:** Document storage confirmation.


## WebSocket API

**Endpoint:** `/ws`

### Workflow

1. **Establish a WebSocket connection**:

```bash
ws://localhost:8080/ws
```

2. **Send a message**:

```json
{
   "event": "life cycle event",
   "data": {
      "query": "example query"
   }
}
```

3. **Receive a response**:

```json
{
   "stack_id": "response_value",
   "result": "",
   "extras": "Dictionary"
}
```


The KISS AI Stack Server provides a robust backend layer with abstracted RAG APIs, lifecycle event handling, and session-based instances, enabling you to focus on functionality without worrying about backend complexity.

