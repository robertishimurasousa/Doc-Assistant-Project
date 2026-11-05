# Vocareum setup

Use this guide when your course provides a Vocareum API key.

## Configure

Complete the [quick start installation](QUICK_START.md), copy `.env.example` to `.env`, and set:

```dotenv
API_PROVIDER=vocareum
OPENAI_API_KEY=your-course-api-key
OPENAI_BASE_URL=https://openai.vocareum.com/v1
DEFAULT_MODEL=gpt-4
DEFAULT_TEMPERATURE=0
```

The endpoint and model are repository defaults. Use your course's assigned values if they differ. The model must support tool calling and structured output through this client.

## Run

```bash
python main.py data/sales_report_q1_2024.txt
```

Enter `What does the report say about monthly revenue?` and inspect the response. Client initialization alone does not verify authentication or remaining budget. A normal model-backed turn uses multiple requests for classification, tool selection, response generation, and memory summarization.

## Troubleshoot

| Symptom | Check |
| --- | --- |
| Missing key | Confirm `.env` is in the repository root with a non-placeholder key. |
| Authentication error | Confirm the key belongs to the endpoint. |
| Model access error | Select a model assigned to your course. |
| Budget or quota error | Check the course allocation. |
| Settings unchanged | Check exported variables and restart the process or notebook kernel. |

For direct OpenAI access, set `API_PROVIDER=openai`, supply the corresponding key, and remove/unset `OPENAI_BASE_URL`. See [configuration](CONFIGURATION.md).
