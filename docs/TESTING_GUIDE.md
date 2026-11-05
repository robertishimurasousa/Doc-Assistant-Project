# Testing guide

The repository combines assertion-based pytest tests, configuration diagnostics, and manual demonstrations. They do not provide the same evidence.

## Install dependencies

Follow the [quick start](QUICK_START.md), then run:

```bash
python -m pip check
```

Testing dependencies are included in `requirements.txt`.

## Focused offline suite

From the repository root:

```bash
python -m pytest tests/test_schemas.py tests/test_agents.py tests/test_agent_logic.py
```

These files exercise schemas, agent functions, routing, mock LLM responses, and message construction without provider requests. The integration-marked cases in `test_agents.py` also use mocks; they do not validate a live model or full persisted application flow.

The selected tests currently pass their assertions but do not meet the configured coverage threshold. The command above therefore exits unsuccessfully at the coverage gate; use the explicit assertion-only command below when that is the intended check.

| File | Role |
| --- | --- |
| `test_schemas.py` | Model validation and defaults. |
| `test_agents.py` | Node behavior, fallback routing, and mock-based node sequences. |
| `test_agent_logic.py` | State, response, and message-format assertions. |
| `test_config.py` | Environment-dependent client diagnostics; some checks print/return results. |
| `test_assistant.py` | Live demonstration that writes sample files under `documents/`. |
| `test_with_sample_data.py` | Live demonstration using bundled data and saved sessions. |
| `check_environment.py` | Legacy environment diagnostic with stale path assumptions. |

## Pytest and coverage configuration

[pyproject.toml](../pyproject.toml) enables verbose output, strict markers, branch coverage, a 50% coverage threshold, a 300-second per-test timeout, and stopping after five failures.

Default reports are `.coverage`, `htmlcov/`, and `coverage.xml`. Review the working tree after testing: some coverage artifacts are already tracked.

For a focused assertion run without the configured coverage gate or reports:

```bash
python -m pytest tests/test_schemas.py tests/test_agents.py tests/test_agent_logic.py -o addopts=""
```

This override is useful during diagnosis; it is not evidence that the default coverage threshold passed.

Do not assume `python -m pytest` over the entire repository is offline. It collects manual demonstrations, and functions in `test_with_sample_data.py` expect an `assistant` argument that is not defined as a pytest fixture. Prefer the explicit file list above.

## Manual provider demonstrations

Configure a real provider as described in [configuration](CONFIGURATION.md), then choose a demonstration:

```bash
python -m tests.test_with_sample_data
python -m tests.test_assistant
```

These commands call the configured provider and write session transcripts. The second command creates or overwrites `documents/sales_data.txt` and `documents/team_info.txt`.

The sample-data script loads six documents: five sample files plus `data/README.md`. Printed completion banners do not establish correctness; query errors can be returned as strings.

For manual acceptance, verify:

1. Q&A agrees with the selected source document.
2. Retrieval logs show the intended documents were searched.
3. Calculations match independently checked arithmetic and calculator usage is visible.
4. Summaries reflect the retrieved content.
5. Follow-up questions retain appropriate context.
6. Saved transcripts can be loaded into a new instance after reloading documents.

The notebook offers another [manual walkthrough](../notebooks/README.md).

## Reporting results

### Documentation validation baseline

On November 5, 2026, validation on macOS with Python 3.14.10 produced:

- A successful fresh installation from `requirements.txt` and a clean `pip check`.
- 57 passing tests in the three-file offline selection.
- 39.50% branch-inclusive coverage for that selection, below the configured 50% gate.
- Successful offline checks of document loading, retrieval, the no-LLM query fallback, calculator output, transcript saving, and session restoration using a temporary directory.
- No live provider acceptance run.

The environment resolved LangChain 1.4.9, LangGraph 1.2.20, langchain-openai 1.7.4, Pydantic 2.14.3, and pytest 9.2.0. These are observed versions, not dependency pins. Coverage used the repository's branch/gate settings with terminal-only reporting and a temporary coverage data file to preserve existing artifacts.

Record the Python and dependency versions, exact command, test outcome, and whether real provider calls occurred. Distinguish mock tests from live acceptance. Do not treat historical coverage files or course checklists as evidence for the current checkout.

See [troubleshooting](TROUBLESHOOTING.md) for known diagnostic limitations.
