# EduAssist AI - LLMOps Assignment 2

## 1. Project Overview

This project evaluates three academic question-answering prompt
versions using a locally hosted Qwen 2.5 1.5B model.

The experiment measures response latency, token consumption,
estimated hosted inference cost and JSON output validity.

MLflow is used for prompt registration, evaluation tracking,
comparison and production alias management.

## 2. Technology Stack

- Python 3.13.9
- Ollama 0.40.14
- Qwen 2.5 1.5B (`qwen2.5:1.5b`)
- MLflow 3.18.0
- SQLite
- JupyterLab
- Pandas
- Requests
- JSON Schema validation

## 3. Model and API Configuration

Inference provider: Local Ollama

Model: qwen2.5:1.5b

API version/endpoint: Ollama REST API `/api/generate`

Base URL: http://localhost:11434

The model runs locally. No cloud API key is required.

Token counts are obtained from Ollama response fields:
- prompt_eval_count
- eval_count

## 4. Pricing Methodology

Local Ollama inference has no per-request cloud API charge.

For economic comparison, estimated hosted inference cost is
calculated using the Featherless Qwen 2.5 1.5B model-class pricing:

- Input: USD 0.04 per million tokens
- Output: USD 0.08 per million tokens
- Currency: USD
- Pricing reference date: 2026-11-05
- Pricing source:
  https://featherless.ai/docs/request-pricing-and-credits

These values represent estimated equivalent hosted inference
charges and not actual local Ollama expenditure.

## 5. Experiment Details

MLflow experiment:
`eduassist_llmops_assignment`

Registered prompt:
`eduassist_academic_qa`

Prompt versions:
- Version 1: Basic
- Version 2: Structured
- Version 3: JSON Output

Five academic questions were evaluated against each version,
resulting in 15 measured requests.

The production alias was assigned to Version 3 based on its
measured performance and structured-output compliance.

## 6. Installation

Install Ollama for Windows from:

https://ollama.com/download/windows

Download the model:

```powershell
ollama pull qwen2.5:1.5b
```

Create and activate a Python environment:

```powershell
python -m venv .venv
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

## 7. Start MLflow

From the project root, run:

```powershell
$ArtifactPath = Join-Path (Get-Location).Path "mlartifacts"

mlflow server `
  --backend-store-uri "sqlite:///mlflow.db" `
  --artifacts-destination "$ArtifactPath" `
  --host 127.0.0.1 `
  --port 5000
```

MLflow UI:

http://127.0.0.1:5000

## 8. Environment Variables

The experiment uses these local service settings:

- MLFLOW_TRACKING_URI=http://127.0.0.1:5000
- MLFLOW_REGISTRY_URI=http://127.0.0.1:5000
- OLLAMA_URL=http://localhost:11434/api/generate
- MODEL_NAME=qwen2.5:1.5b

No API key is required for the local Ollama fallback.

## 9. Reproduction Steps

1. Start Ollama and ensure the Qwen model is available.
2. Start the MLflow tracking server.
3. Activate the Python virtual environment.
4. Launch JupyterLab and open `notebooks/LLMOps_Assignment2.ipynb`.
5. Execute the notebook cells in order.
6. Review the three registered prompt versions and evaluation runs.
7. Review the CSV outputs in the `outputs` directory.
8. Open MLflow Prompts and verify Version 3 has the production alias.

## 10. Output Files

- `outputs/latency_tokens_cost.csv`
- `outputs/latency_statistics.csv`
- `outputs/prompt_comparison.csv`
- `outputs/prompt_evaluation_results.csv`
- `screenshots/`
- `reports/`

## 11. Observations and Limitations

Version 3 achieved an 80% JSON validity rate. One response
reached the configured generation limit, demonstrating the need
for output-length management and post-generation validation.

The cost estimates use published hosted inference rates.
They are not actual charges for local inference.
