# Brain Tumor Classification from MRI Reports with Explainable AI

An NLP-based project that classifies written brain MRI reports into **Non-Tumor**, **Pre-Treatment Tumor**, and **Post-Treatment Tumor** categories.

The project explored traditional machine learning, deep learning, and transformer models, including combinations of text representations and embedding techniques. **BioBERT was selected as the final model for the project implementation**, with **FastAPI** for inference and **LIME** for local prediction explanations.

Developed as a BS Information Technology final-year project at the **University of Sialkot, Pakistan**.

## Problem Statement

Brain MRI reports contain detailed clinical findings written in unstructured text. Manually reviewing and categorizing these reports can be time-consuming, especially when distinguishing non-tumor cases from tumor cases before and after treatment. Differences in terminology and report structure also make automated classification challenging.

This project investigates how natural language processing, machine learning, deep learning, and transformer models can classify MRI report text into three categories: **Non-Tumor**, **Pre-Treatment Tumor**, and **Post-Treatment Tumor**.

The project also addresses prediction interpretability through LIME, which identifies words that positively or negatively contribute to the explained class. A FastAPI service provides access to classification and explanations through text and document uploads.

The aim is to support report organization and review while keeping clinical decisions with qualified healthcare professionals.

## Project Objectives

- Classify brain MRI report text into three tumor/treatment-status categories.
- Compare machine learning, deep learning, and transformer approaches.
- Explore combinations of text representations, embeddings, and classifiers.
- Integrate the selected BioBERT model into an inference API.
- Explain local predictions through contributing words and their weights.
- Support pasted text and uploaded report documents.

## Research Publication

The related research was published in **Scientific Reports**, part of the Nature Portfolio.

**Comparative evaluation of traditional and advanced AI models for classifying brain tumor status from MRI reports**

- **Published:** 25 September 2026
- **Co-author:** Talha Asif
- **Research:** https://www.nature.com/articles/s41598-026-72966-1
- **DOI:** https://doi.org/10.1038/s41598-026-72966-1

This repository shares the BioBERT implementation and its FastAPI/LIME service. The complete comparative experiments and published evaluation protocol are not reproduced by these two notebooks.

## Classification Categories

| Category | Description |
|---|---|
| Non-Tumor | Reports categorized as having no tumor |
| Pre-Treatment Tumor | Reports categorized as tumor cases before treatment |
| Post-Treatment Tumor | Reports categorized as tumor cases following treatment |

**The model analyzes MRI report text, not MRI images.** These categories represent tumor/treatment status rather than tumor subtype or grade.

## Models and Approaches Explored

### Traditional Machine Learning

- Logistic Regression
- Decision Tree
- Random Forest
- Support Vector Machine (SVM)

### Deep Learning

- Recurrent Neural Network (RNN)
- Long Short-Term Memory (LSTM)
- Gated Recurrent Unit (GRU)

### Text Representations and Embeddings

- TF-IDF
- Word2Vec
- GloVe
- FastText

Different combinations of text representations, embeddings, and machine learning/deep learning models were explored and compared.

### Transformer Models

- BERT
- BioBERT
- ClinicalBERT
- DistilBERT

### Final Model

**BioBERT was selected as the final model for our project implementation.** The fine-tuned model was integrated with a FastAPI inference service and LIME explanations.

The code currently available in this repository focuses on **BioBERT training, evaluation, inference, and explainability**. Other model experiments are not currently included.

## Features

- BioBERT fine-tuning for three-class report classification.
- Label normalization and encoding.
- Stratified training/test splitting.
- Accuracy, precision, recall, and F1-score evaluation.
- Confusion matrix visualizations.
- Temperature scaling and confidence reliability diagrams.
- Bootstrapped confusion matrix analysis.
- Model, tokenizer, and label encoder export.
- FastAPI prediction and explanation endpoints.
- PDF, DOCX, and TXT report input.
- Clinical-text extraction and heuristic cleanup.
- LIME explanations showing positive and negative word contributions.
- Keyword-based prediction post-processing.
- Google Colab execution with optional ngrok access.

## Explainable AI with LIME

LIME (**Local Interpretable Model-agnostic Explanations**) helps inspect individual predictions by identifying the top contributing words in a report.

It creates perturbed versions of the input text, obtains model probabilities, and fits a local surrogate model to estimate word contributions for the explained class.

| Contribution | Interpretation |
|---|---|
| Positive weight | Supports the explained class in the local surrogate |
| Negative weight | Pushes the local explanation away from that class |
| Larger absolute weight | Indicates a stronger contribution in the local surrogate |

The API returns contributing words and their signed weights through `token_weights`. An interface can use these values to highlight supporting and opposing words with different colors.

The supplied backend returns the explanation data; it does not itself render colored text highlights.

LIME explanations describe local model behavior. They do not establish clinical causation or guarantee that a prediction is correct.

## Repository Files

| File | Purpose |
|---|---|
| `BioBERT.ipynb` | Model training, evaluation, calibration, and export |
| `API_and_LIME.ipynb` | FastAPI service, document extraction, prediction, and LIME |
| `README.md` | Project documentation |

The API notebook generates three Python modules:

| Module | Purpose |
|---|---|
| `extractor.py` | Document parsing and clinical-text cleanup |
| `lime_engine.py` | Model loading, inference, post-processing, and explanations |
| `app.py` | FastAPI application and request handlers |

The dataset and trained model weights are not included.

## Technology Stack

| Area | Tools |
|---|---|
| Language | Python |
| Model | BioBERT |
| Training | PyTorch, Hugging Face Transformers, Datasets |
| Data processing | Pandas, NumPy, Scikit-learn |
| API | FastAPI, Uvicorn, Pydantic |
| Explainability | LIME |
| Visualization | Matplotlib, Seaborn |
| Document parsing | pdfplumber, PyPDF2, python-docx |
| Storage and execution | Google Drive, Google Colab |
| Optional API tunnel | ngrok |

## Dataset Format

The training notebook expects an Excel file containing:

| Column | Purpose |
|---|---|
| `Description` | English MRI report text |
| `Group` | Classification label |

Labels are normalized and encoded using `LabelEncoder`. Numeric class IDs depend on the input labels, so inference must use the matching exported encoder.

Dataset access and redistribution require the appropriate institutional permissions.

## Model Training

1. Open `BioBERT.ipynb` in Google Colab.
2. Select a GPU runtime.
3. Run the dependency installation cell.
4. Install the additional plotting/export packages:

   ```python
   !pip install matplotlib seaborn joblib --quiet
   ```

5. Mount Google Drive and update the dataset path:

   ```python
   file_path = "/content/drive/MyDrive/FYP data set.xlsx"
   ```

6. Ensure the dataset contains `Description` and `Group`.
7. Run the remaining cells in order.
8. Inspect the class mapping, evaluation results, and plots.
9. Run the final cells to export the model, tokenizer, and label encoder.

### Training Configuration

| Parameter | Value |
|---|---|
| Base model | `dmis-lab/biobert-base-cased-v1.1` |
| Train/test split | Stratified 80% / 20% |
| Random seed | 42 |
| Maximum sequence length | 512 tokens |
| Epochs | 4 |
| Training batch size | 8 |
| Evaluation batch size | 8 |
| Learning rate | `5e-5` |
| Evaluation frequency | Every epoch |
| Final export | Manual export in the final cells |

### Model Export

The default export directory is:

```text
/content/drive/MyDrive/BioBERT_MRI_Model
```

It contains the saved model, tokenizer, and:

```text
label_encoder.joblib
```

Keep these artifacts together to preserve the correct label mapping.

## FastAPI Setup

1. Open `API_and_LIME.ipynb` in Google Colab.
2. Mount Google Drive.
3. Run the dependency installation cells.
4. Install the file-upload dependency and request client:

   ```python
   !pip install python-multipart requests --quiet
   ```

5. In the cell that generates `lime_engine.py`, set the model path:

   ```python
   LOAD_PATH = "/content/drive/MyDrive/BioBERT_MRI_Model"
   ```

   The supplied API notebook originally uses `/content/drive/MyDrive/apimodel`. Update it to match the folder containing your exported model, tokenizer, and label encoder.

6. Run the cells that generate `extractor.py`, `lime_engine.py`, and `app.py`.
7. Start the API:

   ```python
   !nohup uvicorn app:app --host 0.0.0.0 --port 8000 > uvicorn.log 2>&1 &
   ```

8. Skip the `pkill -f uvicorn` cell during startup, because it stops the server.
9. Inspect `uvicorn.log` if startup fails.

The service uses CUDA when available and otherwise runs on CPU.

### Health Check

```python
import requests

base = "http://127.0.0.1:8000"

response = requests.get(base + "/health", timeout=30)
response.raise_for_status()
print(response.json())
```

### Optional ngrok Access

Remove the hardcoded token from the supplied notebook before sharing it. Use a secret prompt instead:

```python
from getpass import getpass
from pyngrok import ngrok

ngrok.set_auth_token(getpass("ngrok auth token: "))
tunnel = ngrok.connect(8000)

base = tunnel.public_url
print(base)
```

Use the current tunnel URL instead of the fixed URL in the notebook’s test cell. The tunnel remains available only while the Colab runtime and server are active.

Interactive API documentation is available at:

```text
<API_BASE_URL>/docs
```

## API Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/health` | Check service status and model device |
| POST | `/predict` | Classify report text |
| POST | `/explain` | Generate a prediction and LIME explanation |
| POST | `/predict_file` | Classify an uploaded report |
| POST | `/explain_file` | Explain an uploaded report |

Successful report responses include a request ID, source metadata, extracted text, and prediction or explanation results.

### Text Prediction

```python
import requests

base = "http://127.0.0.1:8000"

report = (
    "Findings: This synthetic demonstration report describes an "
    "enhancing intracranial mass with surrounding edema. "
    "Impression: Further clinical assessment is required."
)

response = requests.post(
    base + "/predict",
    json={"text": report},
    timeout=120,
)
response.raise_for_status()
print(response.json())
```

The example text is synthetic and does not represent a validated clinical prediction.

Prediction results include:

- `class_id` — predicted class index.
- `label` — predicted category.
- `confidence` — returned confidence score.
- `overridden` — whether keyword rules changed the prediction.
- `all_probs` — original model probabilities in label-encoder order.

### LIME Explanation

```python
response = requests.post(
    base + "/explain",
    json={
        "text": report,
        "num_features": 10,
        "num_samples": 250,
        "max_chars": 1600,
    },
    timeout=300,
)
response.raise_for_status()

result = response.json()

print(result["prediction"])
for contribution in result["explanation"]["token_weights"]:
    print(contribution["token"], contribution["weight"])
```

### File Prediction

```python
with open("synthetic_report.txt", "rb") as report_file:
    response = requests.post(
        base + "/predict_file",
        files={"file": report_file},
        timeout=120,
    )

response.raise_for_status()
print(response.json())
```

### File Explanation

```python
with open("synthetic_report.txt", "rb") as report_file:
    response = requests.post(
        base + "/explain_file",
        params={
            "num_features": 10,
            "num_samples": 250,
            "max_chars": 1600,
        },
        files={"file": report_file},
        timeout=300,
    )

response.raise_for_status()
print(response.json())
```

Supported formats are **PDF, DOCX, and TXT**. PDF extraction reads embedded text; OCR for scanned PDFs is not implemented.

Text endpoints reject empty or very short input with HTTP 400. Unreadable files or insufficient cleaned text return HTTP 422.

## Evaluation and Implementation Notes

- The supplied training notebook has no saved evaluation outputs, so no measured accuracy for that notebook run is stated here.
- The test partition is reused for epoch evaluation, temperature fitting, and final diagnostics. Independent evaluation requires separate validation/calibration data and an untouched test set.
- Patient and duplicate grouping before splitting are not implemented in this notebook.
- The bootstrapped matrix displays mean ± standard deviation across 1,000 resamples, despite the plot’s confidence-interval label.
- The fitted temperature is not exported or loaded by the API; API probabilities use raw softmax outputs.
- Prediction uses up to 512 tokens. LIME uses up to 256 tokens and defaults to the first 1,600 cleaned characters, so long reports can produce different results between endpoints.
- Keyword overrides are heuristic and use substring matching. Their confidence scores are not calibrated probabilities.
- `all_probs` retains the original model probabilities even when an override changes the predicted label.
- LIME explains the neural model for the selected class, rather than the keyword override rule.
- Clinical-text cleanup is heuristic and does not guarantee de-identification.
- Dependencies are unpinned. Record versions from a tested environment for reproducibility.

## Responsible Use

This project is an academic research prototype. Predictions require clinical review and independent validation before clinical use.

Do not commit patient-identifying data, credentials, or ngrok tokens. The supplied API uses permissive CORS and has no authentication or upload-size limits. Add appropriate controls before handling sensitive data.

## Project Team

- **Talha Asif**
- **Rooha Tariq**
- **Hamza Shafiq**

**University of Sialkot — BS Information Technology**

## Citation

Shafiq, H., Masih, A., Asif, T. et al. Comparative evaluation of traditional and advanced AI models for classifying brain tumor status from MRI reports. *Scientific Reports* (2026).

https://doi.org/10.1038/s41598-026-72966-1

## License

No code license has been specified. Code and dataset reuse permissions should be confirmed with the contributors and relevant institutions.
