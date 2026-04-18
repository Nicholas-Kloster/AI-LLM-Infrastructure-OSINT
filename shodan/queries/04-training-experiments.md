# 4. Training, Fine-Tuning & Experiments

_Section verified: April 2026_

Tooling for model training, hyperparameter sweeps, dataset annotation, and experiment tracking. Exposed instances frequently disclose proprietary training data, custom model weights, evaluation prompts, and HuggingFace tokens stored as secrets.

> Tier legend: **T1** unauthenticated by default · **T2** auth often misconfigured / known bypasses · **T3** recon / fingerprint only.

## Fine-Tuning / Training

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Axolotl" port:8080` | T2 | |
| `"unsloth" port:7860` | T2 | |
| `"OpenLLM" port:3000 "bentoml"` | T2 | |
| `"BentoML" port:3000 "/models"` | T2 | |
| `"Ray" "dashboard" port:8265` | T1 | ShadowRay — CVE-2023-48022, unauth RCE, tens of thousands unpatched |
| `"Ray Serve" port:8000 "deployments"` | T1 | Same cluster = same RCE surface |
| `"Determined AI" port:8080 "experiments"` | T2 | |
| `"ClearML" port:8008 "projects"` | T2 | |
| `"SageMaker" "notebook" port:8443` | T2 | |
| `"Lightning AI" port:7501` | T3 | |
| `"LLaMA-Factory" port:7860` | T2 | WebUI fine-tuning, often public |
| `"feast" OR "feature store" port:6566` | T2 | Feast feature store — rare on Shodan, high-value when found |
| `"tecton" OR "feature platform"` | T3 | Enterprise feature store |

**Ray dashboard is the single highest-severity query in this reference.** CVE-2023-48022 (ShadowRay) is unauthenticated RCE via the job submission API, actively exploited since disclosure, and the patch requires operator action (not automatic). A Ray dashboard on the public internet should be treated as already-compromised infrastructure until proven otherwise.

## ML Experiment / Pipeline Tools

| Shodan Query | Tier | Notes |
|---|---|---|
| `"MLflow" port:5000 "experiments"` | T1 | Unauth MLflow — see CVE-2024-37052 through -37060 (RCE via model deserialization) |
| `"MLflow" "registered-models" port:5000` | T1 | Model registry write access |
| `"mlflow" "artifacts" port:5000` | T1 | Artifact store — trained models + experiment outputs |
| `"Kubeflow" port:8080 "pipeline"` | T2 | |
| `"Airflow" "DAG" port:8080` | T2 | RCE via DAG authoring if write access |
| `"Jupyter" port:8888 "notebook"` | T1 | Token-based auth, frequently empty |
| `"Gradio" port:7860` | T2 | Often public by design — authority depends on app |
| `"Streamlit" port:8501` | T2 | Frequently internal dashboards left public |

## Annotation / RLHF / Eval

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Argilla" port:6900` | T2 | |
| `"Label Studio" port:8080 "label"` | T2 | Labeled datasets often sensitive |
| `"Promptfoo" port:3000` | T3 | |
| `"DeepEval" port:8000` | T3 | |
