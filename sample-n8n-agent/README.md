# Base Sample n8n Agent

Author: Manas Ranjan

This repository contains a sample n8n workflow that demonstrates the essential components required to create an agent for the Live Agent Studio. It serves as a template and reference for building new agents.

## Included Workflows

This repository provides two n8n workflow examples:

1. **Base Sample Agent** (`Base_Sample_Agent.json`)
   - Demonstrates how to handle input, output, and conversation history for the Live Agent Studio.
   - Recommended for most scenarios.
   - Offers full control over conversation history management.
   - Utilizes Supabase for storing messages.

2. **Agent Node Sample** (`Agent_Node_Sample_Agent.json`)
   - A simplified version using n8n's built-in "Agent" node.
   - Manages conversation history through the Agent node.
   - Fully compatible with the Live Agent Studio.
   - Suitable for straightforward agent implementations.

## Key Components

1. **Webhook Endpoint**
   - Accepts authenticated POST requests.
   - Processes incoming queries with user and session details.
   - Ensures secure communication via header-based authentication.

2. **Input Handling**
   - Extracts critical fields from incoming requests:
     - `query`: User's question or command.
     - `user_id`: Unique identifier for the user.
     - `request_id`: Tracking ID for the request.
     - `session_id`: Identifier for the current session.

3. **Database Integration**
   - Uses Supabase for message storage (or any Postgres database for the Agent Node variation).
   - Logs user messages and AI responses.
   - Maintains conversation history with metadata.

4. **Response Management**
   - Ensures a consistent response format.
   - Includes success or failure status.
   - Sends structured responses via the webhook.

## Workflow Overview

1. **Webhook Node**
   - Serves as the entry point for requests.
   - Validates authentication headers (optional; can be added when hosted on the Studio).
   - Routes incoming POST requests.

2. **Input Preparation Node**
   - Extracts and formats input data.
   - Validates required fields.
   - Prepares data for further processing.

3. **Database Nodes**
   - "Add User Message to DB": Logs incoming user queries.
   - "Add AI Message to DB": Stores AI-generated responses.
   - For the Agent Node variation, this functionality is handled by the "Agent" node.

4. **Output Preparation**
   - Sets the success status.
   - Formats response data.
   - Ensures a consistent output structure.

## Credentials

1. **Header Authentication**
   - Used for securing webhook communication.

2. **Supabase API**
   - Required for database operations.
   - Manages conversation history.

   These credentials will be replaced with our own when the agent is hosted on the Studio.

## How to Use

1. Import this workflow as a starting point for new agents.
2. Configure the required credentials:
   - Set up header authentication (optional; can be added when hosted on the Studio).
   - Configure the Supabase connection.
3. Customize the workflow by adding:
   - Additional processing nodes.
   - Integrations with specific AI models.
   - Custom business logic.

## Message Format

### Input
```json
{
    "query": "User's question or command",
    "user_id": "unique-user-identifier",
    "request_id": "request-tracking-id",
    "session_id": "conversation-session-id"
}
```

### Output
```json
{
    "success": true,
    "output": "AI response content",
    "data": "Additional response data"
}
```

## Contributing

This agent is part of the oTTomator agents collection. For contributions or issues, please refer to the main repository guidelines.
