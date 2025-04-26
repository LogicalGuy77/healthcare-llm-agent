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

<p align="center">
  <img src="https://github.com/user-attachments/assets/d0a3880c-190f-4d2c-abdf-5f67788187e5" alt="k" />
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/a4910038-b62f-4a65-86f4-081cecdcbd23" alt="2" />
</p>


