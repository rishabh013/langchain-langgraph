## LangGraph Agentic Chatbot Project

This project is a **Streamlit** application that demonstrates how to build **stateful, agentic AI workflows with LangGraph**.  
It includes three main use cases:

- **Basic Chatbot**: Simple conversational agent using a selected Groq LLM.
- **Chatbot With Web**: Chatbot that can search the web using Tavily tools.
- **AI News**: Agent that fetches and summarizes the latest AI news for a selected time frame.

---

## 1. Project Structure (High Level)

- **`app.py`**: Streamlit entry point (`streamlit run app.py`).
- **`src/langgraphagenticai/ui/`**
  - `uiconfigfile.ini`: Config for page title, LLM options, and use cases.
  - `uiconfigfile.py`: Reads the UI config.
  - `streamlitui/loadui.py`: Builds the Streamlit sidebar & inputs.
  - `streamlitui/display_result.py`: Runs the LangGraph and displays results.
- **`src/langgraphagenticai/LLMS/groqllm.py`**: Groq LLM configuration.
- **`src/langgraphagenticai/graph/graph_builder.py`**: Builds different LangGraph graphs per use case.
- **`src/langgraphagenticai/state/state.py`**: Defines the shared state structure.
- **`src/langgraphagenticai/nodes/`**
  - `basic_chatbot_node.py`: Node logic for Basic Chatbot.
  - `chatbot_with_Tool_node.py`: Node logic for chatbot with tools.
  - `ai_news_node.py`: Nodes to fetch, summarize, and save AI news.
- **`src/langgraphagenticai/tools/search_tool.py`**: Tavily web search tool definition.

---

## 2. Requirements & Setup

1. **Create and activate a virtual environment** (optional but recommended).
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Make sure you have the following API keys:

- **Groq API key** (for LLM): create from the Groq console.
- **Tavily API key** (for web search & AI News): create from Tavily.

You can either:

- Enter these keys directly in the Streamlit sidebar inputs, or  
- Export them as environment variables before running:

```bash
export GROQ_API_KEY="your_groq_key"
export TAVILY_API_KEY="your_tavily_key"
```

---

## 3. Running the Application

From the project root:

```bash
streamlit run app.py
```

Then open the URL shown in your terminal (usually `http://localhost:8501`).

In the **left sidebar**:

1. Select **LLM** (currently Groq).
2. Select a **Groq model**.
3. Enter your **GROQ API KEY**.
4. Select a **Usecase**:
   - `Basic Chatbot`
   - `Chatbot With Web`
   - `AI News`
5. For `Chatbot With Web` or `AI News`, also enter your **TAVILY_API KEY**.

---

## 4. Use Cases

### 4.1 Basic Chatbot

- **Usecase**: `Basic Chatbot`  
- **Flow**:
  - User enters a message in the chat input.
  - LangGraph runs a single chatbot node (`BasicChatbotNode`) backed by the Groq model.
  - Response is shown in the chat UI.

### 4.2 Chatbot With Web

- **Usecase**: `Chatbot With Web`  
- **Additional requirement**: `TAVILY_API_KEY`.  
- **Flow**:
  - User enters a message.
  - Graph uses a tool-enabled LLM and a **Tavily** search tool.
  - Tool calls and final answers are streamed back to the UI.

### 4.3 AI News

- **Usecase**: `AI News`  
- **Additional requirement**: `TAVILY_API_KEY`.  
- **Flow**:
  1. In the sidebar, choose a **time frame**: `Daily`, `Weekly`, or `Monthly`.
  2. Click **“🔍 Fetch Latest AI News”**.
  3. The app:
     - Uses Tavily to search for top AI news (India & global) for the selected period.
     - Summarizes the results using the Groq LLM into markdown.
     - Optionally saves the summary to a markdown file (see `ai_news_node.py`).

If AI News does not work:

- Confirm that **TAVILY_API_KEY** is set and valid.
- Confirm that **GROQ_API_KEY** is set and the selected Groq model is available.

---

## 5. Extending the Project

To add new use cases:

1. Create new nodes in `src/langgraphagenticai/nodes/`.
2. Wire them into `GraphBuilder` in `graph_builder.py`.
3. Add them to the UI config in `uiconfigfile.ini` and handle display in `display_result.py`.
