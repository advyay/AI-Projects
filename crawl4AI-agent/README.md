# Pydantic AI: Documentation Crawler and RAG Agent

An advanced documentation crawler and Retrieval-Augmented Generation (RAG) agent developed by Manas Ranjan. This tool leverages Pydantic AI and Supabase to crawl documentation websites, store content in a vector database, and intelligently answer user queries by retrieving and analyzing relevant documentation chunks.

## Key Features

- Crawl and segment documentation websites
- Store content in a vector database powered by Supabase
- Perform semantic search using OpenAI embeddings
- Enable RAG-based intelligent question answering
- Preserve code blocks during processing
- Interactive querying via a Streamlit-based UI
- Accessible as both an API and a web interface

## Prerequisites

- Python 3.11 or higher
- Supabase account and database setup
- OpenAI API key
- Streamlit (for the web interface)

## Installation Steps

1. Clone the repository:
    ```bash
    git clone https://github.com/coleam00/ottomator-agents.git
    cd ottomator-agents/crawl4AI-agent
    ```

2. Install dependencies (using a Python virtual environment is recommended):
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    pip install -r requirements.txt
    ```

3. Configure environment variables:
    - Rename `.env.example` to `.env`
    - Update `.env` with your API keys and settings:
      ```env
      OPENAI_API_KEY=your_openai_api_key
      SUPABASE_URL=your_supabase_url
      SUPABASE_SERVICE_KEY=your_supabase_service_key
      LLM_MODEL=gpt-4o-mini  # or your preferred OpenAI model
      ```

## Usage Instructions

### Database Initialization

Run the SQL commands in `site_pages.sql` to:
1. Create the required tables
2. Enable vector similarity search
3. Configure Row Level Security policies

In Supabase, navigate to the "SQL Editor" tab, paste the SQL commands, and execute them.

### Crawling Documentation

To crawl and store documentation in the vector database, execute:
```bash
python crawl_pydantic_ai_docs.py
```

This process will:
1. Extract URLs from the documentation sitemap
2. Crawl each page and divide content into chunks
3. Generate embeddings and store them in Supabase

### Interactive Web Interface

Launch the Streamlit-based web interface for querying:
```bash
streamlit run streamlit_ui.py
```

Access the interface at `http://localhost:8501`.

## Configuration Details

### Database Schema

The Supabase database uses the following schema:
```sql
CREATE TABLE site_pages (
     id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
     url TEXT,
     chunk_number INTEGER,
     title TEXT,
     summary TEXT,
     content TEXT,
     metadata JSONB,
     embedding VECTOR(1536)
);
```

### Chunking Parameters

Adjust chunking settings in `crawl_pydantic_ai_docs.py`:
```python
chunk_size = 5000  # Number of characters per chunk
```

The chunking logic ensures:
- Code blocks remain intact
- Paragraph and sentence boundaries are respected

## Project Overview

- `crawl_pydantic_ai_docs.py`: Handles documentation crawling and processing
- `pydantic_ai_expert.py`: Implements the RAG agent
- `streamlit_ui.py`: Provides the web interface
- `site_pages.sql`: Contains database setup commands
- `requirements.txt`: Lists project dependencies

## Live Agent Studio Integration

For a production-ready implementation, refer to the `studio-integration-api` directory. It includes the API endpoint used in the Live Agent Studio platform.

## Robust Error Handling

The system is designed to handle:
- Network interruptions during crawling
- API rate limits
- Database connectivity issues
- Errors in embedding generation
- Invalid URLs or unsupported content formats
