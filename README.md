# LlamaIndex Sample Project

A hands-on sample project demonstrating how to use [LlamaIndex](https://github.com/jerryjliu/llama_index) with OpenAI and web page integration in Python.

---

## 🚀 Features

- **OpenAI Integration:** Uses GPT-3.5-turbo for LLM and text-embedding-3-small for embeddings.
- **Web Scraping:** Scrapes and indexes content from web pages (Wikipedia, tech news, etc.).
- **Vector Indexing:** Creates a searchable vector index from documents.
- **Interactive Chatbot:** Query your indexed content via a simple chat interface.
- **REST API:** Full FastAPI implementation with comprehensive endpoints.
- **Minimal Example:** Start with a basic, no-scraping example for quick testing.

---

## 📁 Project Structure

```
llamaindex-sample/
├── main.py                 # Basic LlamaIndex example (Wikipedia scraping)
├── web_scraper_example.py  # Advanced web scraping with chatbot
├── simple_example.py       # Minimal example (no web scraping)
├── api_server.py           # FastAPI REST API server
├── api_client_example.py   # API client example and testing
├── requirements.txt        # Python dependencies
├── env_example.txt         # Environment variables template
├── API_DOCUMENTATION.md    # Comprehensive API documentation
└── README.md               # Project documentation
```

---

## 🛠️ Setup

### 1. Clone or Create the Project Directory

```bash
cd /path/to/your/projects
# If not already created:
mkdir llamaindex-sample
cd llamaindex-sample
```

### 2. Create and Activate a Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Set Up Your OpenAI API Key

1. Copy the example environment file:
   ```bash
   cp env_example.txt .env
   ```
2. Open `.env` in a text editor and add your actual OpenAI API key:
   ```
   OPENAI_API_KEY=your_actual_openai_key_here
   ```

---

## 🎯 Usage

### Minimal Example (No Web Scraping)

```bash
python simple_example.py
```
- Indexes a few sample text documents and lets you run queries.

### Basic Example (Wikipedia Web Scraping)

```bash
python main.py
```
- Scrapes Wikipedia pages about AI and Machine Learning, creates a vector index, and runs sample queries.

### Advanced Example (Tech News Scraping + Chatbot)

```bash
python web_scraper_example.py
```
- Scrapes tech news sites, analyzes content, creates a vector index, and launches an interactive chatbot.

### REST API Server

```bash
python api_server.py
```
- Starts a FastAPI server with comprehensive REST endpoints for RAG operations.
- Access interactive docs at: http://localhost:8000/docs

### API Client Testing

```bash
python api_client_example.py
```
- Tests all API endpoints with sample data.
- Interactive mode: `python api_client_example.py --interactive`

---

## 🔧 Configuration

- **OpenAI Models:**  
  - LLM: `gpt-3.5-turbo`
  - Embeddings: `text-embedding-3-small`
  - You can change these in the `setup_llamaindex()` function in each script.

- **Chunking Settings:**  
  - Chunk Size: 1024 characters  
  - Chunk Overlap: 20 characters  
  - Adjustable in the `Settings` configuration.

- **Web Scraping URLs:**  
  - Change the URLs in the respective functions in `main.py` and `web_scraper_example.py` to scrape different sites.

---

## 💡 Example Queries

Try these when running the chatbot or query scripts:

- "What is artificial intelligence?"
- "How does machine learning work?"
- "What are the latest trends in technology?"
- "Explain the relationship between AI and ML"

---

## 🔍 Understanding the Output

- **Response:** The AI-generated answer.
- **Source nodes:** Number of source documents used.
- **Source details:** Score and text preview for each source.

---

## 🚨 Troubleshooting

- **API Key Error:**  
  - Ensure your `.env` file exists and contains your OpenAI API key.
  - Verify the API key is valid and has sufficient credits.

- **Import Errors:**  
  - Ensure all dependencies are installed: `pip install -r requirements.txt`
  - Check Python version (3.8+ required).

- **Web Scraping Issues:**  
  - Some websites may block scraping. The fallback mechanism creates sample documents if scraping fails.

- **Rate Limiting:**  
  - OpenAI has rate limits; if you hit them, wait and try again.

---

## 🔗 Useful Links

- [LlamaIndex Documentation](https://docs.llamaindex.ai/)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [API Documentation](./API_DOCUMENTATION.md) - Complete API reference
- [BeautifulSoup Documentation](https://www.crummy.com/software/BeautifulSoup/)

---

## 📝 License

This is a sample project for educational purposes. Feel free to modify and use as needed.

## 🤝 Contributing

Feel free to submit issues, feature requests, or pull requests to improve this sample project! 