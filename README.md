# Named Entity Recognition (NER) Hierarchy Project

This project provides a complete pipeline and interactive web interface for training and executing a Named Entity Recognition (NER) model using token-level classification.

## Directory Structure

```text
NER HIERARCHY/
├── README.md               # Project documentation (this file)
└── PDF_70/                 # Primary source directory
    ├── app.py              # Flask server for the interactive web UI
    ├── run_pipeline.py     # Training and evaluation pipeline script
    ├── ner_pipeline.pkl    # Trained serialization model artifact
    ├── templates/          # HTML templates for Flask UI
    │   └── index.html      # Main application page
    └── NAAS_*.{pdf,json,bio} # PDF source documents, JSON annotations, and BIO tags
```

---

## Components

### 1. Training & Evaluation Pipeline (`run_pipeline.py`)

The script [run_pipeline.py](file:///c:/Users/ankit/OneDrive/Desktop/NER%20HIERARCHY/PDF_70/run_pipeline.py) processes the training dataset, builds features, trains the classifier, and evaluates it:

- **BIO Tag Generation**: Translates standard character-offset JSON annotation documents into token-aligned BIO (`B-` for Beginning, `I-` for Inside, `O` for Outside) tag sets saved as `.bio` files.
- **Feature Extraction**: Extracts context-dependent features for every token:
  - Casing information (lower, title, uppercase, digit).
  - Contextual token features (window of size 2, including previous two tokens and next two tokens).
  - Sentence boundary flags (`BOS`/`EOS`).
- **Model Architecture**: Uses a Scikit-Learn `Pipeline` composed of:
  - `DictVectorizer`: Encodes categorical and sparse dictionaries of features into numeric matrices.
  - `LogisticRegression`: Solves the token-level multiclass tagging task.
- **Evaluation**: Computes and logs a classification report containing precision, recall, and f1-score.
- **Serialization**: Saves the completed model to [ner_pipeline.pkl](file:///c:/Users/ankit/OneDrive/Desktop/NER%20HIERARCHY/PDF_70/ner_pipeline.pkl).

### 2. Flask Application (`app.py`)

The Flask backend [app.py](file:///c:/Users/ankit/OneDrive/Desktop/NER%20HIERARCHY/PDF_70/app.py) provides a web application to test the model:

- Serves the frontend via [index.html](file:///c:/Users/ankit/OneDrive/Desktop/NER%20HIERARCHY/PDF_70/templates/index.html).
- Exposes a `/predict` JSON POST endpoint that accepts raw text, segments and tokenizes it, extracts features, runs model prediction, and constructs the extracted spans with their starting/ending character offsets.

---

## Getting Started

### Prerequisites

Create a virtual environment and install the required dependencies (such as `scikit-learn` and `Flask`):

```bash
# Navigate to the PDF_70 directory
cd PDF_70

# Create and activate virtual environment
python -m venv .venv
.venv\Scripts\activate

# Install dependencies
pip install Flask scikit-learn
```

### Running the Pipeline

To re-train the model, run:

```bash
python run_pipeline.py
```

### Launching the Web Interface

To start the Flask development server, run:

```bash
python app.py
```
Open your browser and navigate to `http://localhost:5000` to interact with the UI.
