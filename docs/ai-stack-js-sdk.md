---
layout: document
title: KISS AI Stack - JavaScript Client SDK
---

The KISS AI Stack Client provides an easy-to-use interface for interacting with the KISS AI Stack Server stub, supporting RESTful and WebSocket APIs to manage Stack session lifecycle and execute tasks.

## Features

- REST client for session management, query execution, and document storage.
- WebSocket client for real-time interactions with the Stack's server session.

## Getting Started

<span style="color: red; font-weight: bold;">Nota Bene:</span>  
_It is recommended to use this client in a proxy service for better security._

### Installation

#### 1. Install the `kiss-ai-stack-client` package:

```bash
npm install kiss-ai-stack-client@^0.1.0-a23
```

#### 2. Initialize the client:

**REST API Client**

```javascript
import { RestEvent } from "kiss-ai-stack-client";

// hostname should not include protocol, e.g., example.com, localhost:8080
// secureProtocol: true for HTTPS, false for HTTP
const client = new RestEvent("your-server-hostname", true);
```

**WebSocket-Based Client**

```javascript
import { WebSocketEvent } from "kiss-ai-stack-client";

// hostname should not include protocol, e.g., example.com, localhost:8080
// secureProtocol: true for WSS, false for WS
const client = new WebSocketEvent("your-server-hostname", true);
```

## Usage

### 1. Authorize a Stack Session
Create or refresh a Stack session.

To authorize the session, if you have saved a previous `clientId` and `clientSecret`, send them to get a new access token. Only the `persistent` scope supports this. The `temporary` scope will deactivate the client upon lifecycle ending.

Just send the `scope` only to generate a new client and save the clientId and clientSecret for persistent sessions.

```javascript
// For a temporary session
const session = await client.authorizeStack(undefined, undefined, "temporary");

// Or, for a persistent session
const session = await client.authorizeStack("your-client-id", "your-client-secret");
```

### 2. Bootstrap the Stack
Initialize the Stack session for task execution. This step is mandatory to start the Stack session.

```javascript
const response = await client.bootstrapStack("Hello!");
```
### 3. Generate an Answer
Send a query and receive the Stack's response.

```javascript
const response = await client.generateAnswer("What is the weather today?");
console.log(response);
```

### 4. Store Documents
Upload files with optional metadata for storage.
```javascript
const files = ["path/to/document1.txt", "path/to/document2.pdf"];
const metadata = { category: "example" };

const uploadResponse = await client.storeData(files, metadata);
const summaryResponse = await client.generateAnswer("Give me a summary of example documents");

console.log(summaryResponse);
```

### 5. Destroy the Stack Session
Close the current session. This will clean up stored documents in a temporary session if applicable.

```javascript
const response = await client.destroyStack("Goodbye!");
console.log(response);
```

## Complete example code

```javascript
import { RestEvent } from "kiss-ai-stack-client";

async function run() {
    const client = new RestEvent("localhost:8080", false);
    await client.authorizeStack(undefined, undefined, "temporary");
    await client.bootstrapStack("Hi");
    console.log(await client.generateAnswer("When the world started?"));
    await client.destroyStack("bye");
}

run();

```

<br><br>

___
