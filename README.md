Welcome to the 
# coreskills4ai-coreskills4ai-rag
This project is designed as advanced modular Retrieval-Augmented Generation (RAG) system for instructional purposes. Instructed set up build, experiment with, and understand AI systems that combine document retrieval, embeddings, memory management, and reasoning — starting with free, local and API tools if available to the Virtual Enviorment. 
# CoreSkills4AI RAG Training Module

The system is built to be "future-proof" and will incompas a UI model development overlay for students that prefer no-code enviorment and have full integration into existing embedded system: It defaults to free/local options (e.g., Ollama for embeddings and LLMs), but can easily switch to paid APIs (e.g., OpenAI or Anthropic for advanced reasoning) for professional/production work. This is achieved through a configurable YAML file and loader scripts that automatically detect and bypass unavailable (paid) options.

---

## Project Overview and Purpose

- **What is RAG?**  
  RAG stands for **Retrieval-Augmented Generation**. It's an AI technique that improves LLM responses by "retrieving" relevant documents from a database and "augmenting" the LLM's input with that context. This reduces hallucinations and makes answers more accurate/factual.

- **Why this structure?**  
  The project is organized modularly (e.g., separate folders for ingestion, retrieval, etc.) to teach best practices in software engineering. It goes beyond RAG setups by adding:
  - Multi-tier memory (Letta/Zep)
  - Hybrid embeddings (local Nomic + optional paid)
  - Automatic provider switching
  - Clean separation of concerns

- **Classroom Focus**:  
  Everything runs locally/offline for free if prefered. Built into the cloudshare VM Paid options are bypassed with clear messages.

- **Professional Extension**:  
  Uncomment paid packages in `requirements.txt` and add API keys to `.env` for instant upgrades (e.g., to Claude, Chat-GPT, Co-pilot, Gemeni for better reasoning).

Following the steps below to set up your on local machine. Validation steps ensure everything works before proceeding.

---

## Step 1: System Requirements and Pre-Setup

**Purpose**: Ensure your machine has the basics to run Python and AI tools efficiently. This step validates hardware/software compatibility.

### Hardware
- At least 8GB RAM (+16GB ~ 32BG recommended for smooth Ollama model loading)
- CPU is fine; GPU recommended but optional for faster embeddings/LLMs
- Virtual Machines Available through classrooms @CoreSkills4ai.com 

### OS
- Windows (tested), but works on macOS/Linux with minor adjustments

### Internet
- Needed for initial downloads (Ollama models, packages); available offline after

### Steps

1. **Install Visual Studio Code (VS Code)**  
   - Download from [https://code.visualstudio.com](https://code.visualstudio.com)
   - Why? VS Code integrates everything: code editing, terminal, Git, and extensions like Python for auto-complete/linting.

2. **Install Python 3.12**  
   - Download from [https://www.python.org/downloads/release/python-3127](https://www.python.org/downloads/release/python-3127)
   - Check "Add python.exe to PATH" during install
   - Why? Python is the language for our AI scripts. 3.12.7 (latest version) is stable and compatible with all libraries.

3. **Install Ollama**  
   - Download from [https://ollama.com/download](https://ollama.com/download)
   - Run the installer
   - Why? Ollama runs free, local AI models (e.g., for embeddings and reasoning) on your machine — no cloud costs or internet needed after download.

### Validation
- Open VS Code → New Terminal (Ctrl+`)
- Run: `python --version` → Should show "Python 3.12.7~x"
- Run: `ollama --version` → Should show a version (e.g., 0.1.XX)

---

## Step 2: Clone/Download the Project and Open in VS Code

**Purpose**: Get the project files on your machine. This includes code, configs, and requirements for reproducibility.

### Steps

1. Download the project ZIP or clone via Git:
   ```bash
   git clone https://github.com/FlyguyTestRun/coreskills4ai-coreskills4ai-rag.git

2. In VS Code: File > Open Folder → Select the coreskills4ai-coreskills4ai-rag folder
Why? Opening as a folder lets VS Code manage the project (e.g., search files, run terminals inside it)


Validation

In VS Code terminal, run dir (Windows) or ls (if Git Bash)
You should see folders like config, core, data, etc., and files like requirements.txt, rag.py

## Step 3: Set Up Virtual Environment and Install Dependencies
**Purpose**: Isolate project libraries to avoid conflicts. requirements.txt lists all needed packages — we use free ones by default.
## Steps

1. Install the Python Extension in VS Code:
Extensions (Ctrl+Shift+X) → Search "Python" (Microsoft) → Install
Why? Adds IntelliSense (auto-complete), debugging, and interpreter selection.

2. In VS Code terminal:
`python -m venv venv312
.\venv312\Scripts\activate`

Why? Creates/activates a virtual env — keeps packages local to this project.

3. Upgrade pip and install:
`python -m pip install --upgrade pip
pip install -r requirements.txt`

4. Select Interpreter:
Ctrl+Shift+P → "Python: Select Interpreter" → Choose the one in `venv312\Scripts\python.exe`
Why? Tells VS Code to use this env for running code.

File Purpose: requirements.txt
A list of Python packages/versions needed
`Run pip install -r` to replicate the exact setup
In classroom, it's free, pre-installed in VM; for APIs, uncomment paid lines

Validation

Run 'pip list' — should show installed packages (e.g., langchain, langchain-ollama)
Create `test_install.py:`
`import langchain
print("LangChain imported successfully!")`
Run: python 'test_install.py`. If no errors, success.

## Step 4: Download and Set Up Local AI Models with Ollama
**Purpose**: Provides free, local embeddings (text-to-vector) and LLMs (reasoning) — core to RAG without costs.
## Steps

In terminal:
`Bash
ollama pull nomic-embed-text`

Why? Nomic is a high-quality, open embedding model — turns text into vectors for similarity search in RAG, this will be the default for offline usage embeddings.

Optional (for local reasoning):
`Bash
ollama pull deepseek-coder:6.7b`

Why? DeepSeek-Coder is an open-source LLM optimized for code/reasoning — used for query analysis, answer synthesis in RAG for testing retention generation.

Ensure Ollama runs:
`Bash
ollama serve`
Or just use — it auto-starts in background.

## Validation

Run `ollama list` — should show downloaded models
Test in code: Add to `test_install.py:`
`Python
from langchain_ollama import OllamaEmbeddings
embeddings = OllamaEmbeddings(model="nomic-embed-text")
print(embeddings.embed_query("Test text"))  # Outputs a vector list`

Run it — if no errors and outputs numbers, good.

## Step 5: Understand and Use Project Structure
**Purpose**: Organizes code for modularity/teachability. Each folder has a specific role in the RAG pipeline.

Run this once to create the folder structure (in terminal):
`Bash
# Create directories
"config","core\memory","core\ingestion","core\embeddings","core\retrieval","core\schemas","core\reasoning","data\raw","data\processed","data\embeddings","data\memory","apps\desktop","apps\api","tests\retrieval_quality","tests\memory_stability","scripts","logs","models" | ForEach-Object { New-Item -ItemType Directory -Force -Path $_ }

# Create __init__.py files
@("","core","core\memory","core\ingestion","core\embeddings","core\retrieval","core\schemas","core\reasoning") | ForEach-Object { New-Item -ItemType File -Force -Path "$_\__init__.py" }`

## Folder/Files and Purposes

Path/Purpose
`config`/Stores YAML configs (e.g., models.yaml)
`config/models.yaml`Central "settings" for models/providers. Easy to tweak without code changes. Classroom: Defaults to local Ollama. Pro: Switches to paid with API keys.
`core`/Core logic modules
`core/memory/`/Handles multi-tier memory (Letta for short/long-term, Zep for persistent)
`core/ingestion`/Processes documents (PDFs, videos, audio)
`core/embeddings/`Converts text to vectors
`core/embeddings/embeddings_loader.py`/Smart loader — prefers free Nomic, falls back if paid unavailable. Prints bypass messages for education.
`core/reasoning/`/AI decision layer
`core/reasoning/llm_loader.py`/Smart loader — tries paid LLMs, bypasses to local with messages. Teaches fallback logic.
`core/retrieval/`/Searches/retrieves docs (Haystack-ai pipelines, Chroma/FAISS)
`core/schemas/`/Data models (e.g., Document, Conversation)
`data/Stores`/ raw/processed files, embeddings, memory
`apps/User`/ interfaces (desktop/Streamlit, api/REST)
`tests/Quality`/ checks (retrieval_quality, memory_stability)
`scripts`/Utility tools (e.g., reindex.py)
`logs`/System logs
`models/`Downloaded Ollama models (optional storage)
`__init__.py`/ files/Make folders importable as Python modules (e.g., from core.ingestion import ...)
`rag.py`/Main script (placeholder) — ties everything together
`requirements.txt`/Package list — reproduce env
`venv312/`/Virtual env folder — isolates dependencies
`.env` (create if needed)/Stores API keys (e.g., OPENAI_API_KEY=sk-...). Ignored in Git for privacy.

## Validation

Run dir or tree /F (if tree installed) — check folders exist
In Python REPL (python), run: from core.embeddings.embeddings_loader import get_embeddings — no errors

 ## Step 6: Run and Validate the RAG System
**Purpose**: Test end-to-end to ensure setup works.
## Steps

1. Add `.env` (optional for classroom — leave blank for free mode)
Run a sample in `rag.py` (expand as needed):
`Python
from core.reasoning.llm_loader import get_llm
from core.embeddings.embeddings_loader import get_embeddings

2. llm = get_llm()
embeddings = get_embeddings()
print("LLM and Embeddings loaded successfully!")`

3. Run: `python rag.py`
Expect bypass messages for paid, then success


## Validation

No errors
See selection messages (e.g., "Using local model: ex:deepseek-coder:6.7b")
If issues, check terminal output/logs


## Troubleshooting

Errors?
Check API keys commented out in `requirements.txt`
Ensure Ollama is running (`ollama serve`)
Verify `config/models.yaml` exists and is valid YAML

## Next Steps
**Extend for Pro:**
Uncomment paid packages in requirements.txt (e.g., openai)
Add keys to .env → system auto-detects and upgrades

**Classroom Tips:**
Explain YAML as "app settings file" — edit to experiment
Use `llm_loader.py` and `embeddings_loader.py` as examples of smart fallback design

**Further Reading:**
LangChain Docs
Ollama GitHub
Haystack Documentation



