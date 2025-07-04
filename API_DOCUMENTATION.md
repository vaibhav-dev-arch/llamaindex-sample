# LlamaIndex RAG API Documentation

## 🚀 Overview

The LlamaIndex RAG API provides REST endpoints for Retrieval-Augmented Generation using LlamaIndex and OpenAI. This API allows you to:

- Add documents to a vector index
- Scrape web pages and add them to the index
- Query the index with natural language questions
- Get detailed responses with source attribution
- Manage documents and monitor system status

## 📋 API Endpoints

### Base URL
```
http://localhost:8000
```

### Interactive Documentation
- **Swagger UI**: `http://localhost:8000/docs`
- **ReDoc**: `http://localhost:8000/redoc`

---

## 🔍 Core Endpoints

### 1. Health Check
**GET** `/health`

Check the health and configuration status of the API.

**Response:**
```json
{
  "status": "healthy",
  "openai_configured": true,
  "index_ready": true,
  "message": "API is ready"
}
```

**Example:**
```bash
curl http://localhost:8000/health
```

### 2. Get Index Status
**GET** `/status`

Get current status of the vector index and documents.

**Response:**
```json
{
  "has_index": true,
  "document_count": 5,
  "node_count": 12,
  "is_configured": true
}
```

**Example:**
```bash
curl http://localhost:8000/status
```

---

## 📄 Document Management

### 3. Add Single Document
**POST** `/documents`

Add a single document to the index.

**Request Body:**
```json
{
  "text": "Your document content here...",
  "metadata": {
    "source": "manual",
    "topic": "technology",
    "author": "John Doe"
  }
}
```

**Response:**
```json
{
  "message": "Document added successfully",
  "document_count": 1,
  "node_count": 3,
  "document_id": 0
}
```

**Example:**
```bash
curl -X POST http://localhost:8000/documents \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Python is a programming language.",
    "metadata": {"source": "manual"}
  }'
```

### 4. Add Multiple Documents
**POST** `/documents/batch`

Add multiple documents to the index in a single request.

**Request Body:**
```json
[
  {
    "text": "First document content...",
    "metadata": {"source": "batch1"}
  },
  {
    "text": "Second document content...",
    "metadata": {"source": "batch1"}
  }
]
```

**Response:**
```json
{
  "message": "Added 2 documents successfully",
  "total_documents": 3,
  "node_count": 8
}
```

**Example:**
```bash
curl -X POST http://localhost:8000/documents/batch \
  -H "Content-Type: application/json" \
  -d '[
    {"text": "Document 1 content", "metadata": {"id": 1}},
    {"text": "Document 2 content", "metadata": {"id": 2}}
  ]'
```

### 5. List Documents
**GET** `/documents`

List all documents currently in the index.

**Response:**
```json
[
  {
    "id": 0,
    "text_preview": "Python is a programming language...",
    "metadata": {"source": "manual"},
    "length": 35
  },
  {
    "id": 1,
    "text_preview": "Machine learning is a subset...",
    "metadata": {"source": "batch1"},
    "length": 45
  }
]
```

**Example:**
```bash
curl http://localhost:8000/documents
```

### 6. Clear All Documents
**DELETE** `/documents`

Remove all documents and reset the index.

**Response:**
```json
{
  "message": "All documents cleared and index reset"
}
```

**Example:**
```bash
curl -X DELETE http://localhost:8000/documents
```

---

## 🌐 Web Scraping

### 7. Scrape Web Pages
**POST** `/scrape`

Scrape content from web pages and add to the index.

**Request Body:**
```json
{
  "urls": [
    "https://en.wikipedia.org/wiki/Artificial_intelligence",
    "https://en.wikipedia.org/wiki/Machine_learning"
  ],
  "chunk_size": 1024,
  "chunk_overlap": 20
}
```

**Response:**
```json
{
  "message": "Successfully scraped 2 documents",
  "scraped_count": 2,
  "total_documents": 5,
  "node_count": 15
}
```

**Example:**
```bash
curl -X POST http://localhost:8000/scrape \
  -H "Content-Type: application/json" \
  -d '{
    "urls": ["https://en.wikipedia.org/wiki/Python_(programming_language)"],
    "chunk_size": 1024,
    "chunk_overlap": 20
  }'
```

---

## 🔍 Querying

### 8. Query Index
**POST** `/query`

Ask questions and get AI-generated answers with source attribution.

**Request Body:**
```json
{
  "query": "What is artificial intelligence?",
  "include_sources": true
}
```

**Response:**
```json
{
  "answer": "Artificial intelligence (AI) is a branch of computer science...",
  "sources": [
    {
      "id": 0,
      "score": 0.95,
      "text": "Artificial intelligence (AI) is intelligence demonstrated by machines...",
      "metadata": {
        "source": "wikipedia",
        "url": "https://en.wikipedia.org/wiki/Artificial_intelligence"
      }
    }
  ],
  "query": "What is artificial intelligence?",
  "processing_time": 1.23
}
```

**Example:**
```bash
curl -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{
    "query": "How does machine learning work?",
    "include_sources": true
  }'
```

---

## 🛠️ Usage Examples

### Python Client Example

```python
import requests

# API base URL
API_URL = "http://localhost:8000"

# Add a document
doc_response = requests.post(f"{API_URL}/documents", json={
    "text": "Python is a high-level programming language.",
    "metadata": {"source": "example"}
})

# Query the index
query_response = requests.post(f"{API_URL}/query", json={
    "query": "What is Python?",
    "include_sources": True
})

print(query_response.json()["answer"])
```

### JavaScript/Node.js Example

```javascript
const axios = require('axios');

const API_URL = 'http://localhost:8000';

// Add document
async function addDocument() {
  const response = await axios.post(`${API_URL}/documents`, {
    text: "JavaScript is a programming language.",
    metadata: { source: "example" }
  });
  console.log(response.data);
}

// Query index
async function queryIndex() {
  const response = await axios.post(`${API_URL}/query`, {
    query: "What is JavaScript?",
    include_sources: true
  });
  console.log(response.data.answer);
}
```

### cURL Examples

**Complete workflow:**
```bash
# 1. Check health
curl http://localhost:8000/health

# 2. Add documents
curl -X POST http://localhost:8000/documents \
  -H "Content-Type: application/json" \
  -d '{"text": "Sample document content", "metadata": {"source": "curl"}}'

# 3. Scrape web pages
curl -X POST http://localhost:8000/scrape \
  -H "Content-Type: application/json" \
  -d '{"urls": ["https://en.wikipedia.org/wiki/Python_(programming_language)"]}'

# 4. Query the index
curl -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{"query": "What is Python?", "include_sources": true}'

# 5. Check status
curl http://localhost:8000/status
```

---

## ⚙️ Configuration

### Environment Variables

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=your_openai_api_key_here
```

### Default Settings

- **LLM Model**: `gpt-3.5-turbo`
- **Embedding Model**: `text-embedding-3-small`
- **Chunk Size**: 1024 characters
- **Chunk Overlap**: 20 characters
- **Temperature**: 0.1

---

## 🚨 Error Handling

### Common HTTP Status Codes

- **200**: Success
- **400**: Bad Request (invalid input)
- **500**: Internal Server Error (API key missing, OpenAI errors)

### Error Response Format

```json
{
  "detail": "Error message describing what went wrong"
}
```

### Common Errors

1. **No OpenAI API Key**
   ```json
   {
     "detail": "OpenAI API key not configured"
   }
   ```

2. **No Index Available**
   ```json
   {
     "detail": "No index available. Please add documents first."
   }
   ```

3. **Invalid Request Format**
   ```json
   {
     "detail": "Error adding document: Invalid document format"
   }
   ```

---

## 📊 Performance Considerations

### Response Times
- **Health/Status checks**: < 100ms
- **Document addition**: 1-5 seconds (depends on document size)
- **Web scraping**: 5-30 seconds (depends on number of URLs)
- **Query processing**: 2-10 seconds (depends on query complexity)

### Rate Limiting
- OpenAI API has rate limits
- Consider implementing request queuing for high-volume usage
- Monitor API usage and costs

### Memory Usage
- Index size depends on document count and chunk size
- Consider persisting index to disk for large datasets
- Monitor memory usage in production

---

## 🔧 Development

### Running the API Server

```bash
# Install dependencies
pip install -r requirements.txt

# Start the server
python api_server.py

# Or with uvicorn directly
uvicorn api_server:app --host 0.0.0.0 --port 8000 --reload
```

### Testing the API

```bash
# Run the test client
python api_client_example.py

# Run interactive mode
python api_client_example.py --interactive
```

### Development Mode

```bash
# Start with auto-reload
uvicorn api_server:app --reload --host 0.0.0.0 --port 8000
```

---

## 🚀 Production Deployment

### Docker Example

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
EXPOSE 8000

CMD ["uvicorn", "api_server:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Environment Variables for Production

```env
OPENAI_API_KEY=your_production_api_key
OPENAI_ORG_ID=your_organization_id
```

### Recommended Production Settings

- Use a production WSGI server (Gunicorn)
- Implement proper logging
- Add authentication/authorization
- Use environment-specific configurations
- Monitor API usage and performance
- Implement proper error handling and retries 