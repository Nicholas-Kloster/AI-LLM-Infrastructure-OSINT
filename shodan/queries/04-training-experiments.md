# 4. Training, Fine-Tuning & Experiments

Tooling for model training, hyperparameter sweeps, dataset annotation, and experiment tracking. Exposed instances frequently disclose proprietary training data, custom model weights, evaluation prompts, and HuggingFace tokens stored as secrets.

## Fine-Tuning / Training

| Shodan Query | Notes |
|---|---|
| `"Axolotl" port:8080` | |
| `"unsloth" port:7860` | |
| `"OpenLLM" port:3000 "bentoml"` | |
| `"BentoML" port:3000 "/models"` | |
| `"Ray" "dashboard" port:8265` | |
| `"Ray Serve" port:8000 "deployments"` | |
| `"Determined AI" port:8080 "experiments"` | |
| `"ClearML" port:8008 "projects"` | |
| `"SageMaker" "notebook" port:8443` | |
| `"Lightning AI" port:7501` | |

## ML Experiment / Pipeline Tools

| Shodan Query | Notes |
|---|---|
| `"MLflow" port:5000 "experiments"` | |
| `"MLflow" "registered-models" port:5000` | |
| `"Kubeflow" port:8080 "pipeline"` | |
| `"Airflow" "DAG" port:8080` | |
| `"Jupyter" port:8888 "notebook"` | |
| `"Gradio" port:7860` | |
| `"Streamlit" port:8501` | |

## Annotation / RLHF / Eval

| Shodan Query | Notes |
|---|---|
| `"Argilla" port:6900` | |
| `"Label Studio" port:8080 "label"` | |
| `"Promptfoo" port:3000` | |
| `"DeepEval" port:8000` | |
