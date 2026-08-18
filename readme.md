# Chatbot Using LangGraph & Streamlit

A simple AI chatbot built using **LangGraph**, **LangChain**, **Google Gemini**, and **Streamlit**.
The application supports conversational memory using LangGraph checkpointing and provides a clean chat interface using Streamlit.

## Features

* Interactive chatbot UI using Streamlit
* Google Gemini LLM integration
* LangGraph-based conversational workflow
* Conversation memory using `InMemorySaver`
* Streaming AI responses
* Session-based chat history
* Secure API key management using environment variables
* Deployable on Streamlit Community Cloud

## Tech Stack

* **Python**
* **Streamlit**
* **LangGraph**
* **LangChain**
* **Google Gemini API**
* **python-dotenv**

## Project Structure

```text
Chatbot_Using_Langgraph/
│
├── langgraph_backend.py
├── streamlit_frontend.py
├── requirements.txt
├── .gitignore
└── README.md
```

### `langgraph_backend.py`

Contains the LangGraph workflow and Gemini model configuration.

It is responsible for:

* Defining the chatbot state
* Initializing the Gemini LLM
* Creating the LangGraph workflow
* Managing conversation memory
* Compiling the chatbot graph

### `streamlit_frontend.py`

Contains the Streamlit user interface.

It is responsible for:

* Displaying previous messages
* Accepting user input
* Streaming AI responses
* Maintaining chat history using `st.session_state`

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/umeshsolanki2005/Chatbot_Using_Langgraph.git
cd Chatbot_Using_Langgraph
```

### 2. Create a virtual environment

```bash
python -m venv myenv
```

Activate it on Windows:

```bash
myenv\Scripts\activate
```

For Linux/macOS:

```bash
source myenv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

Example `requirements.txt`:

```txt
streamlit
langgraph
langchain-core
langchain-google-genai
python-dotenv
```

## Environment Variables

Create a `.env` file in the project root:

```env
GOOGLE_API_KEY=your_google_gemini_api_key
```

Do not commit the `.env` file to GitHub.

Make sure `.gitignore` contains:

```gitignore
.env
.env.local
*.env
myenv/
__pycache__/
*.pyc
```

## Run Locally

Start the Streamlit application:

```bash
streamlit run streamlit_frontend.py
```

Then open the local URL shown by Streamlit, usually:

```text
http://localhost:8501
```

## How It Works

The user enters a message through the Streamlit chat interface.

```text
User
  ↓
Streamlit Frontend
  ↓
LangGraph Workflow
  ↓
Google Gemini
  ↓
LangGraph Memory
  ↓
Streaming Response
  ↓
Streamlit Chat UI
```

LangGraph maintains the conversation state using a `thread_id`, allowing the model to receive previous messages from the same conversation.

Example configuration:

```python
config = {
    "configurable": {
        "thread_id": "thread-1"
    }
}
```

## Streaming Responses

The application streams Gemini responses instead of waiting for the complete response.

```python
ai_message = st.write_stream(
    message_chunk.text
    for message_chunk, metadata in chatbot.stream(
        {"messages": [HumanMessage(content=user_input)]},
        config={
            "configurable": {
                "thread_id": "thread-1"
            }
        },
        stream_mode="messages"
    )
    if message_chunk.text
)
```

Using `.text` extracts only the generated text from Gemini's structured message chunks.

## Deployment on Streamlit Community Cloud

### 1. Push the project to GitHub

```bash
git add .
git commit -m "Deploy chatbot"
git push
```

### 2. Create a Streamlit Cloud app

Select:

```text
Repository:
Chatbot_Using_Langgraph

Main file:
streamlit_frontend.py
```

### 3. Add Gemini API Key

In Streamlit Cloud, open:

```text
Manage App
→ Settings
→ Secrets
```

Add:

```toml
GOOGLE_API_KEY = "your_google_gemini_api_key"
```

Never expose your actual API key in the GitHub repository.

## Future Improvements

Possible improvements include:

* Multiple conversation threads
* New chat functionality
* Persistent database-backed chat history
* User authentication
* Sidebar conversation history
* Delete and rename conversations
* RAG-based document chatting
* Tool calling
* Web search integration
* Improved UI and styling
* PostgreSQL or SQLite checkpoint persistence

## Repository

GitHub:

`https://github.com/umeshsolanki2005/Chatbot_Using_Langgraph`

## Author

**Umesh Solanki**

GitHub: `umeshsolanki2005`

## License

This project is intended for learning and educational purposes.
