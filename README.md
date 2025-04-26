# Healthcare Side Effect Agent with Llama 3.1 and Neo4j

This project demonstrates building a conversational agent using LangChain, Llama 3.1 (via NVIDIA AI Endpoints), and a Neo4j graph database to answer questions about drug side effects.

## Description

The agent leverages a knowledge graph stored in Neo4j containing information about drugs, manufacturers, patient cases, and reported side effects. It uses the Llama 3.1 large language model to understand user queries, identify relevant entities (drugs, manufacturers, age groups), and interact with the Neo4j database via a custom tool to retrieve accurate information.

## Features

*   **Natural Language Interaction**: Ask questions about drug side effects in plain English.
*   **Knowledge Graph Integration**: Retrieves data directly from a structured Neo4j graph.
*   **Entity Linking**: Uses Neo4j's full-text search to map user-mentioned drug/manufacturer names to graph entities, handling potential misspellings.
*   **Dynamic Query Generation**: Constructs Cypher queries based on extracted parameters (drug name, age, manufacturer) from the user's question.
*   **Tool-Based Agent**: Utilizes LangChain's agent framework with custom tools for targeted data retrieval.
*   **Powered by Llama 3.1**: Leverages a powerful LLM for understanding and response generation via NVIDIA AI Endpoints.

## Architecture

1.  **User**: Interacts with the agent via natural language queries.
2.  **LangChain Agent**: Orchestrates the process.
    *   **LLM (Llama 3.1 via NVIDIA API)**: Understands the query, determines necessary actions (tool calls), and generates the final response.
    *   **Prompt Template**: Guides the LLM's behavior.
    *   **Tools (`get_side_effects`)**: Functions the LLM can call.
        *   **Entity Linking (`get_candidates`)**: Maps query terms to graph nodes using full-text search.
        *   **Cypher Query Execution**: Interacts with the Neo4j database.
3.  **Neo4j Database**: Stores the structured knowledge graph about drugs, side effects, cases, and manufacturers.

## Setup

1.  **Clone the repository (if applicable)**
2.  **Install Dependencies**:
    ```bash
    pip install --upgrade --quiet langchain-nvidia-ai-endpoints langchain-community neo4j langchain-core neo4j-driver
    ```
3.  **Environment Variables**: Set the following environment variables before running the notebook. You can set these directly in your environment or modify the relevant cells in the `healthcare-agent.ipynb` notebook.
    *   `NEO4J_URI`: Your Neo4j instance URI (e.g., `bolt://localhost:7687`)
    *   `NEO4J_USERNAME`: Your Neo4j username
    *   `NEO4J_PASSWORD`: Your Neo4j password
    *   `NVIDIA_API_KEY`: Your NVIDIA API key for accessing Llama 3.1.

4.  **Neo4j Database Setup**:
    *   Ensure your Neo4j database is running and accessible.
    *   The notebook assumes the existence of `Drug`, `Manufacturer`, `Case`, and `Reaction` nodes with appropriate relationships (`HAS_REACTION`, `IS_PRIMARY_SUSPECT`, `REGISTERED`).
    *   The notebook attempts to create necessary full-text indexes (`drug`, `manufacturer`). Ensure your Neo4j user has permissions to create indexes.

## Usage

1.  Open and run the `healthcare-agent.ipynb` Jupyter Notebook.
2.  Ensure the environment variables for Neo4j and NVIDIA API Key are correctly set within the notebook or your system environment.
3.  Execute the cells sequentially.
4.  The notebook sets up the database connection, LLM, entity linking functions, the `get_side_effects` tool, and the LangChain agent.
5.  Interact with the agent using the `agent_executor.invoke()` method in the final cells, providing your questions as input.

## Example Queries

```python
# Example invocation within the notebook
agent_executor.invoke(
    {
        "input": "What are the most common side effects when using lyrica for people below 35 years old?"
    }
)

agent_executor.invoke(
    {
        "input": "What are the most common side effects for drugs manufactured by acadia?"
    }
)
