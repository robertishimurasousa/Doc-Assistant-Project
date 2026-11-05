# Configuration reference

[src/config.py](../src/config.py) calls `load_dotenv()` when imported. Existing process environment variables take precedence over values loaded from `.env`.

## Environment variables

| Variable | Default in code | Behavior |
| --- | --- | --- |
| `API_PROVIDER` | `vocareum` | Lowercased selector: `openai` or `vocareum`. |
| `OPENAI_API_KEY` | None | Required by `get_openai_client()`; absent/empty values raise `ValueError`. |
| `OPENAI_BASE_URL` | `https://openai.vocareum.com/v1` in the Vocareum branch | Endpoint for Vocareum or an invalid provider. |
| `DEFAULT_MODEL` | `gpt-4` | Model passed by `get_default_llm()`. |
| `DEFAULT_TEMPERATURE` | `0` | Parsed as a float; invalid text raises `ValueError`. |

The model default is application configuration, not a promise of provider access. Select a supported model that accepts this application's tool-calling and structured-output requests.

## Direct OpenAI

```dotenv
API_PROVIDER=openai
OPENAI_API_KEY=your-api-key
DEFAULT_MODEL=gpt-4
DEFAULT_TEMPERATURE=0
```

Remove `OPENAI_BASE_URL` from the template and unset any exported value. The application does not explicitly pass an endpoint in its `openai` branch, but the underlying client can also read environment configuration.

## Vocareum

```dotenv
API_PROVIDER=vocareum
OPENAI_API_KEY=your-course-api-key
OPENAI_BASE_URL=https://openai.vocareum.com/v1
DEFAULT_MODEL=gpt-4
DEFAULT_TEMPERATURE=0
```

Use the key, endpoint, model access, and budget supplied by your course. See [Vocareum setup](VOCAREUM_SETUP.md).

## Selection and overrides

1. An explicit non-`None` `base_url` argument bypasses provider selection.
2. Otherwise, `API_PROVIDER=openai` constructs the client without an explicit endpoint.
3. `API_PROVIDER=vocareum` reads `OPENAI_BASE_URL`, falling back to the Vocareum URL.
4. An unknown provider prints a warning and follows the Vocareum branch.

Provider names are lowercased but whitespace is not stripped. An explicitly empty base URL is different from an absent variable and results in construction without an explicit endpoint.

## Python configuration

```python
from src.config import get_default_llm, get_openai_client

# Reads DEFAULT_MODEL and DEFAULT_TEMPERATURE.
llm = get_default_llm()

# Explicit settings.
custom_llm = get_openai_client(
    model="your-supported-model",
    temperature=0,
    base_url="https://your-compatible-endpoint.example/v1",
)
```

Calling `get_openai_client()` directly uses its argument defaults, `model="gpt-4"` and `temperature=0`, rather than the `DEFAULT_*` variables.

The assistant accepts an injected LLM. The configured adapter is `ChatOpenAI`; the Anthropic package in requirements does not implement an Anthropic provider option.

## Paths

| Setting | Configuration |
| --- | --- |
| Document path | First CLI argument, `/load <path>`, or Python constructor/method. |
| Session directory | `session_dir` constructor argument, default `sessions`; the CLI uses the default. |
| Relative paths | Resolved from the process working directory. |

The session directory is created automatically, but missing parent directories are not created recursively. There are no application environment settings for retrieval depth, session paths, or checkpoint storage.

## Local configuration check

```bash
python -c "from src.config import get_default_llm; get_default_llm(); print('Client constructed; no API request sent.')"
```

This checks local construction, not authentication. Restart the process or notebook kernel after changing configuration.

Keep credentials in `.env` or the process environment. The tracked `.env.example` contains placeholders.
