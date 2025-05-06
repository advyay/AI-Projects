# Base Sample Python Agent

Author: Manas Ranjan

This is a sample Python FastAPI agent that showcases the essential components required to build an agent for the Live Agent Studio. It serves as a template and reference implementation for creating Python-based agents.

This documentation is detailed to provide you with everything needed to build a Python-based agent for the Live Agent Studio. Spend a few minutes reading it carefully, and you'll be ready to get started.

## Overview

This agent provides a foundation for building AI-powered agents that can:
- Handle natural language queries
- Maintain conversation history
- Integrate with external AI models
- Store and retrieve conversation data
- Manage authentication and security

The agent is available in two variants:
1. **Supabase Version** (`sample_supabase_agent.py`): Uses Supabase for database operations
2. **Postgres Version** (`sample_postgres_agent.py`): Uses direct PostgreSQL connection via asyncpg

## Prerequisites

- Python 3.11 or higher
- pip (Python package manager)
- PostgreSQL database or Supabase account
- Basic knowledge of:
  - FastAPI and asynchronous Python
  - RESTful APIs
  - Pydantic models
  - Environment variables
  - PostgreSQL (for Postgres version)

## Core Components

### 1. FastAPI Application (`sample_supabase_agent.py` and `sample_postgres_agent.py`)

The main application is built using FastAPI, offering:

- **Authentication**
  - Bearer token validation via environment variables
  - Secure endpoint protection
  - Customizable security middleware

- **Request Handling**
  - Asynchronous endpoint processing
  - Structured request validation
  - Error handling with appropriate HTTP status codes

- **Database Integration**
  - Supabase connection management
  - Message storage and retrieval
  - Session-based conversation tracking

### 2. Data Models

#### Request Model
```python
class AgentRequest(BaseModel):
    query: str        # User's input text
    user_id: str      # Unique identifier for the user
    request_id: str   # Unique identifier for the request
    session_id: str   # Current conversation session ID
```

#### Response Model
```python
class AgentResponse(BaseModel):
    success: bool     # Indicates if the request was processed successfully
```

### 3. Database Schema

The agent uses Supabase tables with the following structure:

#### Messages Table
```sql
messages (
    id: uuid primary key
    created_at: timestamp with time zone
    session_id: text
    message: jsonb {
        type: string       # 'human' or 'assistant'
        content: string    # The message content
        data: jsonb        # Optional additional data
    }
)
```

## Setup

1. **Clone the Repository**
   ```bash
   git clone https://github.com/coleam00/ottomator-agents.git
   cd ottomator-agents/~sample-python-agent~

   cp .env.example .env
   nano .env  # Edit with your credentials
   ```

2. **Configure Environment Variables**

   > ⚠️ **Note**: For Docker, avoid wrapping environment variable values in quotes, even if they contain special characters.

   #### Supabase Configuration
   Add the following to your `.env` file:
   ```plaintext
   SUPABASE_URL=your-project-url
   SUPABASE_SERVICE_KEY=your-service-key
   API_BEARER_TOKEN=your-token-here
   ```

   #### PostgreSQL Configuration
   Add the following to your `.env` file:
   ```plaintext
   DATABASE_URL=postgresql://user:password@localhost:5432/dbname
   API_BEARER_TOKEN=your-chosen-token
   ```

3. **Create Database Tables**
   For both Supabase and PostgreSQL:
   ```sql
   CREATE EXTENSION IF NOT EXISTS pgcrypto;

   CREATE TABLE messages (
       id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
       created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
       session_id TEXT NOT NULL,
       message JSONB NOT NULL
   );

   CREATE INDEX idx_messages_session_id ON messages(session_id);
   CREATE INDEX idx_messages_created_at ON messages(created_at);
   ```

   > Note: Supabase enables the `pgcrypto` extension by default.

## Installation Methods

### Docker Installation (Recommended)

1. Build the base image:
```bash
cd ../base_python_docker
docker build -t ottomator/base-python:latest .
cd ../~sample-python-agent~
```

2. Build the agent image:
```bash
docker build -t sample-python-agent .
```

3. Run the container:
```bash
docker run -d --name sample-python-agent -p 8001:8001 --env-file .env sample-python-agent
```

The agent will be accessible at `http://localhost:8001`.

### Local Installation (Without Docker)

1. Create and activate a virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

pip install -r requirements.txt
```

2. Run the agent:

For Supabase version:
```bash
uvicorn sample_supabase_agent:app --host 0.0.0.0 --port 8001
```

For PostgreSQL version:
```bash
uvicorn sample_postgres_agent:app --host 0.0.0.0 --port 8001
```

## Configuration

The agent uses environment variables for configuration. These can be set in a `.env` file or via your operating system's environment variable settings.

## Making Your First Request

Test the agent using curl or any HTTP client:

### Supabase Version
```bash
curl -X POST http://localhost:8001/api/sample-supabase-agent \
  -H "Authorization: Bearer your-token-here" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Hello, agent!",
    "user_id": "test-user",
    "request_id": "test-request-1",
    "session_id": "test-session-1"
  }'
```

### Postgres Version
```bash
curl -X POST http://localhost:8001/api/sample-postgres-agent \
  -H "Authorization: Bearer your-token-here" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Hello, agent!",
    "user_id": "test-user",
    "request_id": "test-request-1",
    "session_id": "test-session-1"
  }'
```

## Building Your Own Agent

1. **Fork the Template**
   - Create a copy of this sample agent
   - Rename files and update paths as needed

2. **Customize the Agent Logic**
   - Locate the TODO section in `sample_supabase_agent.py` or `sample_postgres_agent.py`
   - Add your custom agent logic:
     ```python
     async def get_ai_response(query: str, history: List[Dict]) -> str:
         # Add your AI model logic here
         return "AI response"

     @app.post("/api/your-agent", response_model=AgentResponse)
     async def your_agent(request: AgentRequest):
         history = await fetch_conversation_history(request.session_id)
         response = await get_ai_response(request.query, history)
         await store_message(
             session_id=request.session_id,
             message_type="assistant",
             content=response
         )
         return AgentResponse(success=True)
     ```

3. **Add Dependencies**
   - Update `requirements.txt` with new packages
   - Document any external services or APIs

4. **Implement Error Handling**
   ```python
   try:
       result = await your_operation()
   except YourCustomError as e:
       raise HTTPException(
           status_code=400,
           detail=f"Operation failed: {str(e)}"
       )
   ```

## Troubleshooting

Common issues and solutions:

1. **Authentication Errors**
   - Verify the bearer token in the environment
   - Check the Authorization header format
   - Ensure the token matches exactly

2. **Database Connection Issues**
   - For Supabase:
     - Verify credentials
     - Validate table permissions
   - For PostgreSQL:
     - Check DATABASE_URL format
     - Verify user permissions
     - Ensure the database is running and accessible
     - Check if tables are created correctly

3. **Performance Problems**
   - Optimize database queries
   - Consider caching frequently accessed data
   - For PostgreSQL:
     - Monitor connection pool usage
     - Adjust pool size if necessary

## Contributing

This agent is part of the oTTomator agents collection. For contributions or issues, refer to the main repository guidelines.
