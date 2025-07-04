# LlamaIndex Sample Project - Architecture Diagram

## System Architecture Overview

```mermaid
graph TB
    subgraph "User Interface Layer"
        UI[User Input/Queries]
        CLI[Command Line Interface]
    end
    
    subgraph "Application Layer"
        MAIN[main.py]
        SIMPLE[simple_example.py]
        WEB[web_scraper_example.py]
    end
    
    subgraph "Core LlamaIndex Components"
        LLM[OpenAI LLM<br/>gpt-3.5-turbo]
        EMB[OpenAI Embeddings<br/>text-embedding-3-small]
        INDEX[Vector Store Index]
        PARSER[Sentence Splitter]
        QUERY[Query Engine]
    end
    
    subgraph "Data Sources"
        WEBPAGES[Web Pages<br/>Wikipedia, Tech News]
        DOCS[Sample Documents]
        ENV[Environment Variables<br/>.env file]
    end
    
    subgraph "External Services"
        OPENAI[OpenAI API]
        WEBSCRAPER[BeautifulSoup<br/>Web Scraper]
    end
    
    subgraph "Data Processing Pipeline"
        NODES[Document Nodes]
        VECTORS[Vector Embeddings]
        CHUNKS[Text Chunks]
    end
    
    %% User interactions
    UI --> CLI
    CLI --> MAIN
    CLI --> SIMPLE
    CLI --> WEB
    
    %% Main application flow
    MAIN --> LLM
    MAIN --> EMB
    MAIN --> INDEX
    MAIN --> PARSER
    MAIN --> QUERY
    
    %% Data sources
    WEBPAGES --> WEBSCRAPER
    DOCS --> MAIN
    DOCS --> SIMPLE
    ENV --> MAIN
    ENV --> SIMPLE
    ENV --> WEB
    
    %% External services
    OPENAI --> LLM
    OPENAI --> EMB
    WEBSCRAPER --> WEBPAGES
    
    %% Data processing
    WEBPAGES --> NODES
    DOCS --> NODES
    NODES --> PARSER
    PARSER --> CHUNKS
    CHUNKS --> EMB
    EMB --> VECTORS
    VECTORS --> INDEX
    INDEX --> QUERY
    QUERY --> UI
    
    %% Styling
    classDef appLayer fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef coreLayer fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef dataLayer fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef externalLayer fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef processLayer fill:#fce4ec,stroke:#880e4f,stroke-width:2px
    
    class MAIN,SIMPLE,WEB appLayer
    class LLM,EMB,INDEX,PARSER,QUERY coreLayer
    class WEBPAGES,DOCS,ENV dataLayer
    class OPENAI,WEBSCRAPER externalLayer
    class NODES,VECTORS,CHUNKS processLayer
```

## Data Flow Architecture

```mermaid
flowchart TD
    subgraph "Phase 1: Setup & Configuration"
        A[Load Environment Variables] --> B[Configure OpenAI LLM]
        B --> C[Configure OpenAI Embeddings]
        C --> D[Set Global Settings]
    end
    
    subgraph "Phase 2: Data Ingestion"
        E[Web Scraping] --> F[Create Documents]
        G[Sample Documents] --> F
        F --> H[Document Processing]
    end
    
    subgraph "Phase 3: Index Creation"
        H --> I[Text Chunking]
        I --> J[Generate Embeddings]
        J --> K[Create Vector Index]
    end
    
    subgraph "Phase 4: Query Processing"
        L[User Query] --> M[Query Embedding]
        M --> N[Vector Similarity Search]
        N --> O[Retrieve Relevant Nodes]
        O --> P[Generate Response]
        P --> Q[Return Answer + Sources]
    end
    
    D --> E
    D --> G
    K --> L
    
    %% Styling
    classDef setupPhase fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef ingestPhase fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef indexPhase fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef queryPhase fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    
    class A,B,C,D setupPhase
    class E,F,G,H ingestPhase
    class I,J,K indexPhase
    class L,M,N,O,P,Q queryPhase
```

## Component Interaction Diagram

```mermaid
sequenceDiagram
    participant User
    participant App as Application
    participant LlamaIndex as LlamaIndex Core
    participant OpenAI as OpenAI API
    participant Web as Web Scraper
    participant Index as Vector Index
    
    User->>App: Run script (main/simple/web)
    App->>App: Load .env configuration
    
    alt Web Scraping Mode
        App->>Web: Scrape URLs
        Web->>App: Return documents
    else Simple Mode
        App->>App: Create sample documents
    end
    
    App->>LlamaIndex: Setup LLM & Embeddings
    LlamaIndex->>OpenAI: Configure API connection
    
    App->>LlamaIndex: Create documents
    LlamaIndex->>LlamaIndex: Parse into nodes
    LlamaIndex->>OpenAI: Generate embeddings
    OpenAI->>LlamaIndex: Return vectors
    LlamaIndex->>Index: Store vectors
    
    loop Query Processing
        User->>App: Submit query
        App->>LlamaIndex: Process query
        LlamaIndex->>Index: Search similar vectors
        Index->>LlamaIndex: Return relevant nodes
        LlamaIndex->>OpenAI: Generate response
        OpenAI->>LlamaIndex: Return answer
        LlamaIndex->>App: Format response
        App->>User: Display answer + sources
    end
```

## File Structure and Dependencies

```mermaid
graph TD
    subgraph "Project Root"
        ROOT[llamaindex-sample/]
    end
    
    subgraph "Core Application Files"
        MAIN[main.py<br/>Main application with<br/>Wikipedia scraping]
        SIMPLE[simple_example.py<br/>Minimal example<br/>with sample docs]
        WEB[web_scraper_example.py<br/>Advanced scraping<br/>with chatbot]
    end
    
    subgraph "Configuration Files"
        REQ[requirements.txt<br/>Python dependencies]
        ENV[.env<br/>Environment variables]
        ENV_EX[env_example.txt<br/>Environment template]
    end
    
    subgraph "Documentation"
        README[README.md<br/>Project documentation]
    end
    
    subgraph "Virtual Environment"
        VENV[venv/<br/>Python virtual environment]
    end
    
    %% Dependencies
    MAIN --> REQ
    SIMPLE --> REQ
    WEB --> REQ
    MAIN --> ENV
    SIMPLE --> ENV
    WEB --> ENV
    ENV --> ENV_EX
    
    %% File relationships
    ROOT --> MAIN
    ROOT --> SIMPLE
    ROOT --> WEB
    ROOT --> REQ
    ROOT --> ENV
    ROOT --> ENV_EX
    ROOT --> README
    ROOT --> VENV
    
    %% Styling
    classDef appFiles fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef configFiles fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef docFiles fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef envFiles fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    
    class MAIN,SIMPLE,WEB appFiles
    class REQ,ENV,ENV_EX configFiles
    class README docFiles
    class VENV envFiles
```

## Technology Stack

```mermaid
graph LR
    subgraph "Frontend/Interface"
        CLI[Command Line Interface]
    end
    
    subgraph "Backend Framework"
        PYTHON[Python 3.8+]
    end
    
    subgraph "Core Libraries"
        LLAMA[LlamaIndex 0.9.48]
        OPENAI[OpenAI 1.12.0]
        DOTENV[python-dotenv 1.0.1]
    end
    
    subgraph "Web Scraping"
        BEAUTIFULSOUP[BeautifulSoup4 4.12.3]
        REQUESTS[Requests 2.31.0]
    end
    
    subgraph "AI/ML Services"
        GPT35[GPT-3.5-turbo]
        EMBEDDINGS[text-embedding-3-small]
    end
    
    subgraph "Data Processing"
        VECTOR[Vector Store Index]
        CHUNKING[Text Chunking]
        PARSING[Document Parsing]
    end
    
    CLI --> PYTHON
    PYTHON --> LLAMA
    PYTHON --> OPENAI
    PYTHON --> DOTENV
    PYTHON --> BEAUTIFULSOUP
    PYTHON --> REQUESTS
    
    LLAMA --> VECTOR
    LLAMA --> CHUNKING
    LLAMA --> PARSING
    
    OPENAI --> GPT35
    OPENAI --> EMBEDDINGS
    
    %% Styling
    classDef interface fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef backend fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef core fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef scraping fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    classDef ai fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef data fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    
    class CLI interface
    class PYTHON backend
    class LLAMA,OPENAI,DOTENV core
    class BEAUTIFULSOUP,REQUESTS scraping
    class GPT35,EMBEDDINGS ai
    class VECTOR,CHUNKING,PARSING data
``` 