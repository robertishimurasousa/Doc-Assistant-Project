# Sample documents

This directory contains five example documents for exploring retrieval, summaries, and arithmetic.

| File | Contents | Example request |
| --- | --- | --- |
| [sales_report_q1_2024.txt](sales_report_q1_2024.txt) | Monthly sales, customer metrics, regions, and forecasts. | What does the Q1 report say about monthly revenue? |
| [team_structure.txt](team_structure.txt) | Team members, projects, roles, and hiring plans. | Summarize the team structure. |
| [financial_overview.txt](financial_overview.txt) | Revenue, expenses, cash flow, and financial ratios. | What expenses are listed in the financial overview? |
| [product_catalog.json](product_catalog.json) | Products, prices, categories, and commercial information. | Which products appear in the catalog? |
| [customer_feedback.csv](customer_feedback.csv) | Customer ratings and feedback. | Summarize the customer feedback. |

## Load one document

From the repository root:

```bash
python main.py data/sales_report_q1_2024.txt
```

The sales report lists January revenue of $50,000, February revenue of $60,000, and March revenue of $70,000. Their sum is $180,000 and their average is $60,000. These values provide a simple arithmetic check.

## Load the directory

```bash
python main.py data
```

This loads six documents: five sample files and this README. Markdown files are part of the loader's supported formats. To load only the five samples in Python:

```python
from pathlib import Path
from src.assistant import DocumentAssistant

assistant = DocumentAssistant()
for path in sorted(Path("data").iterdir()):
    if path.suffix in {".txt", ".json", ".csv"}:
        assistant.load_documents(str(path))

print(assistant.get_stats()["num_documents"])  # 5
```

This example only loads documents. Pass `llm=get_default_llm()` when constructing the assistant to enable model-backed queries, as shown in the [quick start](../docs/QUICK_START.md).

## Retrieval behavior

Files are read as UTF-8 text. JSON and CSV are not parsed into records. Each file is a single document, and search ranks substring occurrences of query terms. Loading a file twice creates two entries.

Use the samples for demonstrations; they are not a validated business dataset. See [architecture](../docs/IMPLEMENTATION.md) for retrieval limitations.
