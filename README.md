# CLINICAL REPORT SIMILARITY FINDER

## HI 744 – Text Retrieval & Its Application in Biomedicine

**Execution Environment: Google Colab**

---

### Authors
* **Atharva Pradeep Vaishnav**
* **Rushikesh Ganesh Deshpande**

**Course:** HI 744

---

### Project Overview

This project implements a **clinical report similarity retrieval system** that retrieves the most similar clinical case reports given a free-text clinical query. The system compares:
1.  **Lexical Retrieval** (TF-IDF)
2.  **Semantic Retrieval** (ClinicalBERT)

The task is formulated as a document ranking (information retrieval) problem rather than a text classification task. In addition to the retrieval pipelines, the project includes an interactive user interface (UI) that allows users to submit clinical queries and view retrieval results from both TF-IDF and ClinicalBERT side-by-side. The UI also presents an LLM-generated summary and explanation describing why the retrieved clinical cases are similar to the input query, improving interpretability and user understanding.

All experiments and the UI were developed and executed in Google Colab due to the size of the dataset and GPU requirements.

---

### Important Notes (Please Read First)

#### NGROK TOKEN REQUIREMENT (FOR UI DEMO)

To run the **Streamlit demo UI** inside Google Colab, you **must** provide an **ngrok authentication token**.

**Steps:**

1.  Create a free ngrok account at: [https://ngrok.com/](https://ngrok.com/)
2.  Copy your ngrok authtoken from the ngrok dashboard.
3.  Open the notebook cell number **28**.
4.  Replace the placeholder line: `"YOUR_NGROK_TOKEN_HERE"` with your token.
    * *e.g.:* `!ngrok config add-authtoken 3d...`

> **Note:** Ngrok is required **only** for the Streamlit UI demo. The core retrieval experiments do **NOT** require ngrok.

---

### Task Description

#### Input
* A free-text clinical case report or clinical note (e.g., a patient case description).
* Internally, the system also takes a corpus of clinical case reports sampled from the PMC-Patients dataset.

#### Output
* A **ranked list** of the top-k most similar clinical case reports from the corpus.
* Similarity scores for each retrieved report.
* LLM-generated **summary/explanation** describing why the retrieved cases are similar to the query.
* Evaluation metrics comparing retrieval methods: **Precision@5** and **Recall@5**.

#### Relevance Definition
A retrieved document is considered relevant if it shares the **same disease label** as the query document.

---

### Dataset

| Feature | Details |
| :--- | :--- |
| **Source** | PMC-Patients dataset (de-identified clinical case reports) |
| **Full Dataset Size** | Approximately 161,000 reports |
| **Sampling Strategy** | For reproducibility, internal sampling is performed inside the notebook. No absolute file paths are hard-coded. All file handling is done dynamically within Google Colab. |

---

### Notebook Files Included

This project includes two Google Colab notebooks.

#### 1. MAIN EXPERIMENT NOTEBOOK (Used for Final Report)

| Feature | Details |
| :--- | :--- |
| **Filename** | `Text_Retrieval_Final_Project_10000_Sample_Cases.ipynb` |
| **Description** | Uses **10,000** sampled clinical case reports. All results reported in the final project PDF are generated from this notebook. |
| **Includes** | TF-IDF lexical retrieval, ClinicalBERT semantic retrieval, Evaluation (Precision@5, Recall@5), Clustering and visualization (PCA and t-SNE), LLM-based query expansion. |
| **Important** | This notebook is computationally heavier. Intended for full experimental runs and final reporting. |

#### 2. QUICK DEMO NOTEBOOK

| Feature | Details |
| :--- | :--- |
| **Filename** | `Text_Retrieval_Final_Project_100Sample_Cases.ipynb` |
| **Description** | Uses **100** sampled clinical case reports. |
| **Designed for** | Fast execution, Demonstration without long runtime. Produces the same pipeline outputs as the main notebook, but on a smaller scale. |
| **Important** | Results from this notebook are **NOT** used in the final report. |

#### Changing the Sample Size

The dataset sample size is configurable directly in the notebook:
* **Where to change:** In **cell 7** - Baseline Retrieval Using TF-IDF and BM25.
* **Example:**
    ```python
    nlp = spacy.load("en_core_web_sm")
    df = df_all.sample(n=100, random_state=42).reset_index(drop=True)
    print("Using sample of dataset:", df.shape)
    ```

---

### How to Run the Project in Google Colab

**Step 1: Open the Notebook**
* Upload the desired notebook (`.ipynb` file) to Google Colab.

**Step 2: Run all cells**
* Run all cells in sequence.

**Step 3: For final output open external link for UI (last cell)**
* Follow the link provided by the ngrok tunnel (if using the UI demo).

**Step 4: Provide a query in the text box titled "Enter a clinical note"**

**Example query:**
> A 58-year-old male with long-standing alcohol use presents with progressive abdominal distension, jaundice, and right upper quadrant pain. Examination shows ascites, spider angiomas, and hepatomegaly. Laboratory studies reveal elevated AST and ALT, hyperbilirubinemia, and prolonged INR. CT imaging demonstrates a nodular liver with portal hypertension consistent with cirrhosis.

 **Important Note:** If it appears that Google Colab is unresponsive when you click on "Run all", we recommend you to run the cells one by one.

---

### Expected Output

After running the notebook, the following outputs are produced:

* Top-k similar clinical reports for sample queries.
* **Precision@5** and **Recall@5** scores for:
    * TF-IDF
    * ClinicalBERT
* PCA and t-SNE embedding visualizations.
* LLM-based query expansions.

---

### Reproducibility Notes

* Random seeds are fixed where applicable.
* No external APIs are required for core functionality.
* LLM usage (FLAN-T5) is run locally.
* Final report results correspond **ONLY** to the 10,000-sample main notebook.
