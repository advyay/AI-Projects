# Voiceflow Dialog API Integration

Author: Manas Ranjan

> For comprehensive details about the Voiceflow Dialog API and its integration with Voiceflow agents, refer to the [official Voiceflow Dialog API documentation](https://docs.voiceflow.com/reference/overview).

This integration ensures seamless connectivity between Voiceflow agents and the Live Agent Studio. Its standout feature is the zero-configuration setup—**once an agent is built in Voiceflow, it is instantly ready for use in the Live Agent Studio without requiring additional setup or customization**.

This document outlines the implementation details of how Voiceflow agents integrate with the Live Agent Studio. While no code modifications are necessary to use your Voiceflow agents, understanding the underlying implementation can be helpful for advanced use cases.

## Overview

This integration offers:
- Instant deployment of Voiceflow agents to Live Agent Studio
- Real-time conversation management via Voiceflow's Dialog API
- Automatic tracking of message history
- Support for rich responses (text, buttons, carousels - planned enhancement)
- Secure API authentication
- Session management to maintain conversation context

## Prerequisites

- Python 3.11 or later
- pip (Python package manager)
- Supabase account (for storing conversation data)
- Voiceflow account with an API key
- Basic knowledge of:
  - FastAPI and asynchronous Python
  - RESTful APIs
  - Voiceflow Dialog API

## Core Components

### 1. FastAPI Application (`voiceflow_integration.py`)

The integration is built using FastAPI and includes:

- **Voiceflow Integration**
  - Communication with the Dialog API
  - Session state management
  - Handling of rich responses

- **Authentication**
  - Bearer token validation
  - Secure endpoint protection
  - API key management

- **Database Integration**
  - Supabase connection for storing messages
  - Tracking conversation history
  - Managing session data

### 2. Data Models

#### Request Model
```python
class AgentRequest(BaseModel):
    query: str        # User's input text or JSON action
    user_id: str      # Unique identifier for the user
    request_id: str   # Unique identifier for the request
    session_id: str   # Current conversation session ID
```

#### Response Model
```python
class AgentResponse(BaseModel):
    success: bool     # Indicates if the request was successfully processed
```

### 3. Database Schema

The integration uses a Supabase table with the following structure:

#### Messages Table
```sql
messages (
    id: uuid primary key
    created_at: timestamp with time zone
    session_id: text
    message: jsonb {
        type: string       # 'human' or 'assistant'
        content: string    # Message content
        data: jsonb        # Voiceflow response data
    }
)
```

## Frontend Integration

The `VoiceflowFrontendComponent.tsx` demonstrates how to integrate Voiceflow traces from the Dialog API into the Live Agent Studio frontend. This component serves as an example of creating custom frontend elements to display rich responses from your agent using the `data` field of the AI response instead of plain text.

### Key Features
- Handles various Voiceflow trace types (text, choice, knowledgeBase)
- Renders interactive buttons for choice responses
- Processes structured message data from Voiceflow's Dialog API
- Maintains conversation context through session management

### Example Usage
```typescript
interface VoiceflowTrace {
  type: string;
  payload: {
    message?: string;
    slate?: {
      content: Array<{
        children: Array<{
          text: string;
        }>;
      }>;
    };
    buttons?: VoiceflowButton[];
  };
}

// Component renders different UI elements based on trace type
const renderTrace = (trace: VoiceflowTrace) => {
  switch (trace.type) {
    case 'text':
      return <p>{trace.payload.message}</p>;
    case 'choice':
      return <ButtonGroup buttons={trace.payload.buttons} />;
    // ... handle other trace types
  }
};
```

## Setup

1. **Environment Variables**
   Create a `.env` file with the following:
   ```
   SUPABASE_URL=your_supabase_url
   SUPABASE_SERVICE_KEY=your_supabase_key
   API_BEARER_TOKEN=your_api_token
   VOICEFLOW_AGENT_API_KEY=your_voiceflow_api_key
   ```

2. **Local Development**
   ```bash
   # Install dependencies
   pip install -r requirements.txt

   # Run the server
   python voiceflow_integration.py
   ```

3. **Docker Deployment**
   ```bash
   # Build the container
   docker build -t voiceflow-integration .

   # Run the container
   docker run -d --name voiceflow-integration -p 8001:8001 --env-file .env voiceflow-integration
   ```

## Using Your Voiceflow Agent

1. Create and publish your agent in Voiceflow.
2. Copy the Dialog API key from Voiceflow.
3. Add the API key to your environment variables.
4. Your agent is now ready to use in the Live Agent Studio!

## API Endpoints

### POST /api/sample-voiceflow-agent
Handles communication between Live Agent Studio and your Voiceflow agent.

**Request:**
```json
{
    "query": "Hello!",
    "user_id": "user123",
    "request_id": "req123",
    "session_id": "sess123"
}
```

**Response:**
```json
{
    "success": true
}
```

