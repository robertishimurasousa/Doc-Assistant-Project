# Repository guidance for WARP

Use the maintained documentation when working in this repository:

- [README](README.md): project overview and interfaces.
- [Quick start](docs/QUICK_START.md): environment and setup.
- [Architecture](docs/IMPLEMENTATION.md): actual graph, tools, retrieval, and persistence.
- [API reference](docs/API_REFERENCE.md): methods and response behavior.
- [Testing](docs/TESTING_GUIDE.md): offline checks and live demonstrations.
- [Contributing](CONTRIBUTING.md): change and documentation conventions.

## Common commands

Run from the repository root with the virtual environment activated:

```bash
python -m pip install -r requirements.txt
python main.py data/sales_report_q1_2024.txt
python -m pytest tests/test_schemas.py tests/test_agents.py tests/test_agent_logic.py -o addopts=""
git diff --check
```

The pytest override runs assertions without the default coverage gate. Consult the testing guide for the measured baseline and coverage command.

## Implementation conventions

Keep prompts in `src/prompts.py`, schemas in `src/schemas.py`, and graph nodes in `src/agent.py`. LLM and tool instances reach nodes through `config["configurable"]`.

Tool selection uses the model, but Python explicitly executes returned calls. There is one tool-selection round. Q&A wraps plain model text in `AnswerResponse`; summary and calculation routes request their own structured schemas.

The public query API returns strings. Graph checkpoints are in memory, while JSON files store only transcripts and metadata. Retrieval is keyword-based and loading is additive.

Keep documentation and project metadata in English. Check generated session and coverage files before staging changes.
