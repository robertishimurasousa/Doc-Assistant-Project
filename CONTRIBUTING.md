# Contributing

Contributions should keep the educational workflow easy to understand and its documentation aligned with the code.

## Set up

1. Follow the [quick start](docs/QUICK_START.md).
2. Review the [architecture](docs/IMPLEMENTATION.md) and [API reference](docs/API_REFERENCE.md).
3. Create a branch for a focused change.

Run the application from the repository root. Keep API keys in the ignored `.env` file.

## Make a change

- Keep prompts in `src/prompts.py`, response models in `src/schemas.py`, and graph nodes in `src/agent.py`.
- Inject the LLM and tools through graph configuration so node behavior can be tested with mocks.
- Add or adjust meaningful tests when changing routing, schemas, tools, or persistence.
- Update the relevant guide whenever commands, configuration, or user-visible behavior change.
- Write documentation, examples, and project metadata in English.
- Preserve historical notes as historical context; put current behavior in maintained guides.

## Validate

```bash
python -m pip check
python -m pytest tests/test_schemas.py tests/test_agents.py tests/test_agent_logic.py -o addopts=""
git diff --check
git status --short
```

The command above runs assertions without the default coverage gate. See [testing](docs/TESTING_GUIDE.md) for the coverage baseline and before running the entire suite or live demonstrations. Review generated transcripts, sample files, and coverage artifacts before staging changes.

For documentation-only work, check relative links, command paths, code examples, and claims against the implementation. A live API demonstration is not necessary for wording changes.

## Submit or report an issue

Describe the problem, the resulting behavior, and the validation performed. State whether checks used mocks or a real provider, and include any remaining limitations.

For a bug report, include reproduction steps, Python/dependency versions, expected behavior, actual behavior, and a redacted error. Use a small sample document rather than private source material.

There is no automated release or deployment workflow checked into this repository, and no `LICENSE` file is currently included.
