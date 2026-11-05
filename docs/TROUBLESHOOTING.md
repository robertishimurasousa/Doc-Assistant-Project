# Troubleshooting

Run commands from the repository root with the virtual environment activated.

| Symptom | Explanation and action |
| --- | --- |
| `No module named 'src'` | Use the repository root as the working directory. Run demonstrations with `python -m tests.test_assistant` or `python -m tests.test_with_sample_data`. |
| Missing dependency or import failure | Install `requirements.txt` in the active interpreter and run `python -m pip check`. There is no dependency lockfile. |
| `OPENAI_API_KEY not found` | Copy the template to `.env` and configure the key; restart the application. |
| `LLM not configured` in an answer | The assistant has no LLM. Document loading alone does not enable model-backed answers. |
| Authentication, model, or quota error | Verify the key, endpoint, model access, and provider allocation. Client construction does not authenticate. |
| CLI says “Vocareum” while using OpenAI | The startup success text is hardcoded. Verify actual configuration rather than relying on the banner. |
| Configuration edits have no effect | Existing environment variables override dotenv values. Restart the process or kernel and review exported variables. |
| Zero documents loaded | Check the path, UTF-8 encoding, and lowercase supported extensions. Missing paths can silently load nothing in the Python API. |
| Six documents appear for five sample files | Directory loading includes `data/README.md`. Load individual files to exclude documentation. |
| Document counts grow after `/load` | Loading appends without deduplication. Restart or clear the retriever through Python. |
| No relevant documents | Search uses substring counts, not semantic matching. Use terms actually present in the documents. |
| Summary/calculation output looks like model fields | The public API stringifies those response models. Inspect graph state using the [API reference](API_REFERENCE.md). |
| Sources contain `document_reader` or `calculator` | Source fields can contain tool names; they are not guaranteed document citations. |
| Calculator was not called | Its use is prompt-directed. There is no mandatory calculator step or repeated tool-planning loop. |
| Session file does not appear | Verify directory permissions and inspect `save_session()` errors. Query execution does not propagate a failed save as an exception. |
| A resumed session has no documents | JSON transcripts do not store the document collection. Reload the documents in the new instance. |
| Context is inconsistent across turns | History is resubmitted to an additive graph field, and summary fields are reset per query. See [architecture](IMPLEMENTATION.md). |
| Pytest reports a missing `assistant` fixture | The sample-data demonstration is being collected as a test. Use the explicit offline file list in [testing](TESTING_GUIDE.md). |
| Environment checker cannot find root files | `tests/check_environment.py` resolves some paths relative to `tests/`. Its Python 3.8 check is also not a dependency compatibility guarantee. |

## Inspect local configuration

```bash
python -m pip check
python -c "from src.config import get_default_llm; get_default_llm(); print('Client constructed; no API request sent.')"
```

Avoid using `tests/test_config.py` output as a shareable credential diagnostic: it can print a prefix of the API key. Redact credentials, private paths, and document contents from issue reports.

## Check arithmetic independently

```python
from src.tools import create_calculator_tool

calculator = create_calculator_tool()
print(calculator.invoke({"expression": "(50000 + 60000 + 70000) / 3"}))
# Result: 60000.0
```

This tests the calculator directly without an LLM. It does not prove that a natural-language request caused the graph to call it.
