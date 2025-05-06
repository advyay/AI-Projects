# OpenAI Agents SDK Demo

This repository showcases examples of using the OpenAI Agents SDK to create intelligent travel planning agents with progressively advanced features.

## Author

Manas Ranjan

## Project Structure

- `v1_basic_agent.py` - A simple agent that generates a haiku about recursion
- `v2_structured_output.py` - A travel agent producing structured outputs using Pydantic models
- `v3_tool_calls.py` - A travel agent integrating weather forecasting tools
- `v4_handoffs.py` - A travel agent utilizing specialized sub-agents for flights and hotels
- `v5_guardrails_and_context.py` - A travel agent with budget constraints and user preferences
- `v6_streamlit_agent.py` - A Streamlit-based web interface for the travel agent with chat memory

## Setup

1. Install the required dependencies:

```bash
pip install -r requirements.txt
```

2. Create a `.env` file with your OpenAI API key:

```
OPENAI_API_KEY=your_api_key_here
MODEL_CHOICE=gpt-4o-mini  # or another model of your choice
```

## Running the Examples

### Basic Agent (v1)

Run the basic agent example:

```bash
python v1_basic_agent.py
```

This will execute a simple agent that generates a haiku about recursion.

### Structured Output Agent (v2)

Run the structured output travel agent example:

```bash
python v2_structured_output.py
```

This demonstrates the use of Pydantic models to generate structured travel plans, including destinations, activities, and budget details.

### Tool Calls Agent (v3)

Run the tool calls travel agent example:

```bash
python v3_tool_calls.py
```

This version incorporates a weather forecasting tool to provide weather details for travel destinations.

### Handoffs Agent (v4)

Run the handoffs travel agent example:

```bash
python v4_handoffs.py
```

This version introduces specialized sub-agents for flight and hotel recommendations, showcasing agent delegation.

### Guardrails and Context Agent (v5)

Run the guardrails and context travel agent example:

```bash
python v5_guardrails_and_context.py
```

This version includes:
- Budget validation to ensure travel plans are realistic
- User context for storing preferences like airlines and hotel amenities

[Optional] Follow the [Logfire setup instructions](https://logfire.pydantic.dev/docs/#logfire) (free to get started) for tracing in this version and version 6. The example will still work without Logfire, but tracing won't be available.

### Streamlit Chat Interface (v6)

Launch the Streamlit web interface:

```bash
streamlit run v6_streamlit_agent.py
```

This will start a web server and open a browser window with the travel agent chat interface. Features include:

- Persistent chat history within a session
- User preference management in the sidebar
- Well-formatted responses for various travel details
- Support for conversation memory across multiple interactions

## Environment Variables

The following environment variables can be configured in your `.env` file:

- `OPENAI_API_KEY` (required): Your OpenAI API key
- `MODEL_CHOICE` (optional): The OpenAI model to use (default: gpt-4o-mini)

## Features Demonstrated

1. **Basic Agent Configuration (v1)**
   - Simple agent setup and execution

2. **Structured Output (v2)**
   - Using Pydantic models for structured responses
   - Organized travel planning

3. **Tool Calls (v3)**
   - Integration of external tools for data retrieval
   - Weather forecasting functionality

4. **Agent Handoffs (v4)**
   - Specialized agents for flights and hotels
   - Delegation to domain-specific sub-agents

5. **Guardrails and Context (v5)**
   - Budget validation for realistic travel plans
   - User context for personalized recommendations
   - Sorting results based on user preferences

6. **Chat Interface (v6)**
   - Persistent conversation history
   - User preference management
   - Formatted responses for various output types
   - Thread management for ongoing conversations

## Notes

This is a demonstration project and uses simulated data for weather, flights, and hotels. For production use, integration with real APIs would be required.
