---
layout: document
title: Core - Build a Stack
---

This guide provides instructions for setting up and running the AI Stack using the provided configuration and example Python code.



## Prerequisites

1. **Python Environment**:
   - Python 3.12 or higher installed.
   - A virtual environment set up for dependencies.

2. **Dependencies**:
   Install core: `pip install kiss-ai-stack-core~=0.1.0a23`

3. **Configuration File**:
   Prepare a YAML configuration file (refer to the `AI Stack Configuration Guide`) for defining the stack's structure and tools.

4. **API Keys**:
   Ensure you have the necessary API keys for AI models, embeddings, and vector database integrations. Replace `<your-api-key>` in the configuration with the actual keys.


## Step 1: Setting Up the Stack

Use the `AgentStack` class to bootstrap, interact with, and destroy stacks. Below is an example of how to initialize and interact with a stack.



### Python Code Example

```python
# run_stack.py

import asyncio
from kiss_ai_stack import AgentStack


async def main():
    # Step 1: Bootstrap the stack
    await AgentStack.bootstrap_stack('my_stack')

    # Step 2: Query the stack
    query = 'How are you?'
    response = await AgentStack.generate_answer('my_stack', query)
    print(response.answer)

    # Step 3: Destroy the stack
    await AgentStack.destroy_stack('my_stack', True)


if __name__ == "__main__":
    # Use asyncio to run the main async function
    asyncio.run(main())
```



## Step 2: Understanding the `AgentStack` Class

The `AgentStack` is a centralized class for managing AI stacks. Below are its key features and methods:


### Key Features

- **Thread-Safe Management**: Ensures safe access to stacks in concurrent environments.
- **Flexible Stack Initialization**: Supports temporary (stateless) and persistent modes.
- **Query Processing**: Handles natural language queries and structured requests.
- **Document Storage**: Stores files and metadata for retrieval-augmented generation (RAG).



### Methods Overview

#### 1. `bootstrap_stack`
Initializes a stack session with optional persistence.
```python
await AgentStack.bootstrap_stack(stack_id: str, temporary: Optional[bool] = True)
```

#### 2. `generate_answer`
Processes a query and generates a structured response.
```python
response = await AgentStack.generate_answer(stack_id: str, query: Union[str, Dict, List])
```

#### 3. `store_data`
Stores documents and metadata in the vector database.
```python
stored_documents = await AgentStack.store_data(stack_id: str, files: List[str], metadata: Optional[Dict[str, Any]] = None)
```

#### 4. `get_stack`
Retrieves an active stack instance by its ID.
```python
stack = AgentStack.get_stack(stack_id: str)
```

#### 5. `destroy_stack`
Closes a stack session and cleans up resources.
```python
await AgentStack.destroy_stack(stack_id: str, cleanup: bool = False)
```



## Step 3: Running the Script

1. Save the provided Python code to a file, e.g., `run_stack.py`.
2. Run the script:
   ```bash
   python run_stack.py
   ```
3. The output should display the stack's response to the query.



## Troubleshooting

### Common Issues

1. **Stack Initialization Failure**:
   - Check the YAML configuration file for missing or incorrect entries.
   - Ensure API keys and model configurations are valid.

2. **Query Processing Errors**:
   - Verify the stack is correctly initialized before sending queries.
   - Check logs for detailed error messages.

3. **Document Storage Issues**:
   - Ensure file paths are correct and accessible.
   - Verify the vector database is running and reachable.


### Debugging Logs
Logs are generated to help debug issues. Check the `LOG` output for details.
