# Notebook demo

[document_assistant_demo.ipynb](document_assistant_demo.ipynb) demonstrates document loading, Q&A, calculations, summaries, conversation context, retrieval, and session management.

## Start the notebook

Complete the [quick start](../docs/QUICK_START.md), including provider configuration. With the virtual environment active, run from the repository root:

```bash
python -m ipykernel install --user --name doc-assistant --display-name "Python (Document Assistant)"
cd notebooks
python -m notebook document_assistant_demo.ipynb
```

Select the **Python (Document Assistant)** kernel, then execute cells in order.

The first code cell assumes the kernel's working directory is `notebooks/` and computes the repository root using `Path.cwd().parent`. Check the printed project root before proceeding. Editors that start kernels in the repository root require adjusting that cell to use `Path.cwd()` instead.

## What to expect

- The demo loads `data/`, including its README: six documents in total.
- Model-backed cells call the configured provider and consume its quota.
- Sessions are written to the repository's `sessions/` directory.
- Q&A returns answer text; calculation and summary outputs may display model fields.
- Printed “success” messages are demonstrations, not assertion-based acceptance tests.

Stored notebook outputs represent earlier runs. Restart the kernel and rerun cells to evaluate current behavior. Before sharing changes, inspect outputs for credentials, private document text, and local paths.

## Troubleshooting

For missing modules, select the environment in which requirements were installed. For incorrect paths, check the kernel working directory. After changing `.env`, restart the kernel.

See [configuration](../docs/CONFIGURATION.md), [testing](../docs/TESTING_GUIDE.md), and [troubleshooting](../docs/TROUBLESHOOTING.md).
