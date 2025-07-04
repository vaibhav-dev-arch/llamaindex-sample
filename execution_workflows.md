# LlamaIndex Sample Project - Execution Workflows

## 1. Simple Example Workflow (simple_example.py)

```mermaid
flowchart TD
    A[Start simple_example.py] --> B[Load .env file]
    B --> C{API Key Present?}
    C -->|No| D[Display Error Message]
    C -->|Yes| E[Configure OpenAI LLM<br/>gpt-3.5-turbo]
    E --> F[Configure OpenAI Embeddings<br/>text-embedding-3-small]
    F --> G[Set Global Settings]
    G --> H[Create Sample Documents]
    H --> I[Document 1: Python programming]
    H --> J[Document 2: Machine learning]
    H --> K[Document 3: LlamaIndex framework]
    H --> L[Document 4: OpenAI models]
    I --> M[Create Vector Index]
    J --> M
    K --> M
    L --> M
    M --> N[Create Query Engine]
    N --> O[Run Test Queries]
    O --> P[Q: What is Python?]
    O --> Q[Q: How does ML work?]
    O --> R[Q: What is LlamaIndex?]
    O --> S[Q: Tell me about OpenAI]
    P --> T[Display Response]
    Q --> T
    R --> T
    S --> T
    T --> U[End]
    D --> U
```

## 2. Main Application Workflow (main.py)

```mermaid
flowchart TD
    A[Start main.py] --> B[Load .env file]
    B --> C{API Key Present?}
    C -->|No| D[Display Error Message]
    C -->|Yes| E[Setup LlamaIndex Configuration]
    E --> F[Configure OpenAI LLM & Embeddings]
    F --> G[Set Chunk Size: 1024<br/>Chunk Overlap: 20]
    G --> H[Define Sample URLs]
    H --> I[Wikipedia: AI Article]
    H --> J[Wikipedia: ML Article]
    I --> K[Load Web Pages]
    J --> K
    K --> L[BeautifulSoup Web Reader]
    L --> M[Create Documents from Web Content]
    M --> N[Parse Documents into Nodes]
    N --> O[Sentence Splitter<br/>Chunk Size: 1024]
    O --> P[Create Vector Index]
    P --> Q[Run Sample Queries]
    Q --> R[Q: What is AI?]
    Q --> S[Q: How does ML relate to AI?]
    Q --> T[Q: What are AI applications?]
    R --> U[Query Engine Processing]
    S --> U
    T --> U
    U --> V[Display Response + Sources]
    V --> W[Show Source Nodes Info]
    W --> X[End]
    D --> X
```

## 3. Advanced Web Scraper Workflow (web_scraper_example.py)

```mermaid
flowchart TD
    A[Start web_scraper_example.py] --> B[Load .env file]
    B --> C{API Key Present?}
    C -->|No| D[Display Error Message]
    C -->|Yes| E[Setup LlamaIndex Configuration]
    E --> F[Configure OpenAI LLM & Embeddings]
    F --> G[Set Global Settings]
    G --> H[Define Tech News URLs]
    H --> I[TechCrunch]
    H --> J[The Verge]
    H --> K[Wired]
    I --> L[Scrape Web Content]
    J --> L
    K --> L
    L --> M{Scraping Successful?}
    M -->|No| N[Create Fallback Documents]
    M -->|Yes| O[Process Scraped Content]
    N --> P[Sample AI/ML Documents]
    O --> Q[Analyze Content]
    P --> Q
    Q --> R[Display Content Statistics]
    R --> S[Total Documents Count]
    R --> T[Total Characters Count]
    R --> U[Average Document Length]
    S --> V[Create Search Index]
    T --> V
    U --> V
    V --> W[Parse into Nodes]
    W --> X[Create Vector Index]
    X --> Y[Start Chatbot Interface]
    Y --> Z[Display Chatbot Welcome]
    Z --> AA[Wait for User Input]
    AA --> BB{User Input}
    BB -->|quit/exit/q| CC[End Chatbot]
    BB -->|Valid Query| DD[Process Query]
    DD --> EE[Query Engine]
    EE --> FF[Generate Response]
    FF --> GG[Display Bot Response]
    GG --> AA
    CC --> HH[End]
    D --> HH
```

## 4. Query Processing Pipeline

```mermaid
flowchart LR
    A[User Query] --> B[Query Preprocessing]
    B --> C[Generate Query Embedding]
    C --> D[Vector Similarity Search]
    D --> E[Retrieve Top-K Nodes]
    E --> F[Context Assembly]
    F --> G[Send to LLM with Context]
    G --> H[Generate Response]
    H --> I[Format Response]
    I --> J[Add Source Information]
    J --> K[Return to User]
    
    subgraph "Vector Search Process"
        D --> D1[Calculate Cosine Similarity]
        D1 --> D2[Rank by Similarity Score]
        D2 --> D3[Select Top Results]
    end
    
    subgraph "LLM Processing"
        G --> G1[Combine Query + Context]
        G1 --> G2[Send to GPT-3.5-turbo]
        G2 --> G3[Generate Answer]
    end
    
    subgraph "Response Formatting"
        I --> I1[Extract Answer Text]
        I1 --> I2[Identify Source Nodes]
        I2 --> I3[Calculate Relevance Scores]
    end
    
    D --> D1
    G --> G1
    I --> I1
```

## 5. Error Handling and Fallback Mechanisms

```mermaid
flowchart TD
    A[Application Start] --> B{Environment Check}
    B -->|Missing .env| C[Display Setup Instructions]
    B -->|Invalid API Key| D[Display API Key Error]
    B -->|Valid Setup| E[Proceed with Execution]
    
    E --> F{Web Scraping}
    F -->|Success| G[Process Scraped Content]
    F -->|Failure| H[Create Fallback Documents]
    F -->|Rate Limited| I[Wait and Retry]
    
    G --> J[Continue Processing]
    H --> J
    I --> F
    
    J --> K{OpenAI API Calls}
    K -->|Success| L[Generate Response]
    K -->|Rate Limited| M[Implement Backoff]
    K -->|API Error| N[Display Error Message]
    
    L --> O[Display Results]
    M --> K
    N --> P[Graceful Degradation]
    
    C --> Q[End with Instructions]
    D --> Q
    O --> R[Continue or Exit]
    P --> R
    Q --> S[Application End]
    R --> S
```

## 6. Data Transformation Pipeline

```mermaid
flowchart TD
    subgraph "Input Sources"
        A1[Web Pages]
        A2[Sample Documents]
        A3[User Queries]
    end
    
    subgraph "Text Processing"
        B1[HTML Parsing<br/>BeautifulSoup]
        B2[Text Extraction]
        B3[Content Cleaning]
    end
    
    subgraph "Document Creation"
        C1[Create Document Objects]
        C2[Add Metadata]
        C3[Document Validation]
    end
    
    subgraph "Chunking & Parsing"
        D1[Sentence Splitter]
        D2[Chunk Size: 1024]
        D3[Overlap: 20 chars]
        D4[Create Nodes]
    end
    
    subgraph "Vectorization"
        E1[Generate Embeddings]
        E2[text-embedding-3-small]
        E3[Vector Storage]
    end
    
    subgraph "Index Creation"
        F1[Vector Store Index]
        F2[Similarity Search]
        F3[Query Engine]
    end
    
    A1 --> B1
    A2 --> B2
    A3 --> F3
    
    B1 --> B2
    B2 --> B3
    B3 --> C1
    C1 --> C2
    C2 --> C3
    C3 --> D1
    D1 --> D2
    D2 --> D3
    D3 --> D4
    D4 --> E1
    E1 --> E2
    E2 --> E3
    E3 --> F1
    F1 --> F2
    F2 --> F3
``` 