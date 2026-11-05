# Tests and demonstrations

The maintained instructions are in the [testing guide](../docs/TESTING_GUIDE.md).

For the offline assertion suite, run from the repository root:

```bash
python -m pytest tests/test_schemas.py tests/test_agents.py tests/test_agent_logic.py -o addopts=""
```

This explicitly disables configured coverage options. On November 5, 2026, the selected 57 tests passed; their measured branch-inclusive coverage was 39.50%, below the repository's 50% gate.

The directory also contains manual provider demonstrations and environment diagnostics. Do not assume collecting every file with pytest is offline or supported. See the guide for commands, side effects, and acceptance criteria.
