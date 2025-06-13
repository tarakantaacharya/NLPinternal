# College Information Chatbot

This project is a conversational chatbot designed to answer queries about colleges using Retrieval-Augmented Generation (RAG). It leverages `ChromaDB` for semantic search, `SentenceTransformers` for encoding queries, and a large language model (LLM) via HuggingFace Transformers to generate refined, human-like answers. The chatbot can be used interactively in a Jupyter/Colab notebook.

## Features

- **Conversational Chatbot:** Engages users in a loop, answering queries about college information.
- **Retrieval-Augmented Generation (RAG):** Uses semantic search to find relevant college data and refines responses with an LLM for accuracy and fluency.
- **Vector Database with ChromaDB:** Stores and retrieves relevant documents efficiently.
- **Flexible LLM Backend:** Easily switch the LLM model (e.g., Flan-T5, Llama2, Mistral, etc.).
- **Customizable Query Handling:** Distinguishes between specific and open-ended user queries for tailored responses.
- **Google Drive Integration:** Supports persistent storage via Google Drive in Colab environments.

## Dependencies

- Python 3.x
- [ChromaDB](https://github.com/chroma-core/chroma)
- [SentenceTransformers](https://www.sbert.net/)
- [transformers](https://huggingface.co/docs/transformers/index)
- [streamlit](https://streamlit.io/) (optional for app deployment)
- Google Colab (for mounting Google Drive)

Install required packages:
```bash
pip install chromadb streamlit sentence-transformers transformers
```

## Usage

### 1. Install Dependencies

Make sure all required libraries are installed. If running in Colab, use:
```python
!pip install chromadb streamlit
```

### 2. Mount Google Drive (Colab Only)

```python
from google.colab import drive
drive.mount('/content/drive')
```

### 3. Load Embedding and Generation Models

```python
from transformers import pipeline
generator = pipeline("text2text-generation", model="google/flan-t5-large")

from sentence_transformers import SentenceTransformer
model = SentenceTransformer("all-MiniLM-L6-v2")
```

### 4. Initialize ChromaDB Client and Collection

```python
import chromadb
chroma_client = chromadb.PersistentClient(path="/content/drive/MyDrive/college_rag_db")
collection = chroma_client.get_or_create_collection(name="college_info")
```

### 5. Define Helper Functions

- `refine_response(user_query, chroma_results)`: Uses LLM to generate structured answers.
- `search_college_info(query, top_k=3)`: Runs semantic search and response refinement.

### 6. Chat Loop

```python
while True:
    text = input("Enter your query (or type 'Bye' or 'Exit' to quit): ")
    if text.lower() in ["bye", "exit"]:
        break
    else:
        search_college_info(text)
```

## Example

```
Enter your query (or type 'Bye' or 'Exit' to quit): Where is R V R AND J C COLLEGE OF ENGINEERING located?
Response: GUNTUR, Andhra Pradesh.
```

## Customization

- Change the LLM model in the `generator` pipeline as needed.
- Adjust the ChromaDB path and collection for your dataset.
- Modify the keywords in `refine_response` for different types of queries.

## License

This repository is for educational purposes. Please check individual package licenses for usage in production.

## Acknowledgements

- [ChromaDB](https://www.trychroma.com/)
- [HuggingFace Transformers](https://huggingface.co/)
- [SBERT Sentence Transformers](https://www.sbert.net/)
