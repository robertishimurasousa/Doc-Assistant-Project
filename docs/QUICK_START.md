# Quick start

This tutorial takes you from a checkout to a document question in the terminal.

## 1. Prepare Python

You need Git, Python with `venv` and `pip`, and network access for installation. Model-backed queries also require a provider key and a model supporting tool calling and structured output.

The repository does not declare a minimum Python version or lock dependencies. Installation and offline checks were verified on Python 3.14.10 on macOS on November 5, 2026. This is a tested environment, not a minimum-version declaration. Use `python3.14` below if your `python3` points to an older system interpreter.

```bash
git clone https://github.com/robertishimurasousa/Doc-Assistant-Project.git
cd Doc-Assistant-Project
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m pip check
```

On Windows PowerShell, activate with `.venv\Scripts\Activate.ps1` instead.

Run the remaining commands from the repository root. This project runs from source; `pyproject.toml` configures tests rather than package builds.

## 2. Configure a provider

```bash
cp .env.example .env
```

For direct OpenAI access, replace the template settings with:

```dotenv
API_PROVIDER=openai
OPENAI_API_KEY=your-api-key
DEFAULT_MODEL=gpt-4
DEFAULT_TEMPERATURE=0
```

Remove `OPENAI_BASE_URL` from `.env` and unset any exported value for direct OpenAI access. For a course key, follow [Vocareum setup](VOCAREUM_SETUP.md). See [configuration](CONFIGURATION.md) for precedence and custom endpoints.

Creating the client does not verify credentials, model availability, or provider budget.

## 3. Load a sample document

```bash
python main.py data/sales_report_q1_2024.txt
```

The terminal should report one loaded document and start a session. Enter:

```text
What does the Q1 sales report say about monthly revenue?
Summarize the Q1 sales report.
Calculate 50000 + 60000 + 70000.
/stats
/quit
```

The standalone arithmetic example evaluates to `180000`. Natural-language responses vary, and the calculation route does not guarantee a calculator call. Inspect tool logs when evaluating tool behavior.

Each query attempts to save its transcript in `sessions/`. Workflow errors may be returned as answer strings, so a printed response alone does not prove success.

## 4. Load your documents

Create a `documents/` directory and add UTF-8 text files, then run:

```bash
python main.py ./documents
```

Directory scans support lowercase `.txt`, `.md`, `.json`, and `.csv` extensions and include nested directories. JSON and CSV files are read as text, without record parsing.

Within a running session:

```text
/load ./documents
/stats
```

Loading appends documents and can create duplicates. Start a fresh application or call `assistant.retriever.clear_documents()` in Python to replace the collection.

## Next steps

- [Python API](API_REFERENCE.md): session restoration, in-memory documents, and structured state.
- [Notebook guide](../notebooks/README.md): interactive walkthrough.
- [Testing](TESTING_GUIDE.md): local checks without provider calls.
- [Troubleshooting](TROUBLESHOOTING.md): installation and query problems.
