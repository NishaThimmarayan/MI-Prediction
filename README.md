# Myocardial Infarction Risk Prediction System

A Flask-based machine learning web application that predicts a patient's **Myocardial Infarction (MI)** risk from clinical details, vital signs, cardiac biomarkers, ECG findings, and lifestyle-related risk factors.

> ⚠️ **Important:** This project is built for educational and demonstration purposes only. It must not be used as a replacement for a qualified medical professional, emergency care, or clinical diagnosis.

---

## Overview

Heart attack risk assessment often requires reviewing several patient details, including vital signs, blood tests, ECG findings, medical conditions, and lifestyle risk factors.

This project uses a trained **XGBoost classification model** to process these inputs and provide:

- A predicted probability of MI risk
- A risk classification: **Low**, **Medium**, or **High**
- An interactive web interface for entering patient information
- Input validation to reduce incorrect user entries
- A live-monitoring interface with normal and high-risk visual feedback

---

## Features

- Predicts risk of Myocardial Infarction from patient data
- Uses a trained XGBoost machine learning model
- Accepts clinical inputs such as:
  - Age and gender
  - Heart rate
  - Systolic and diastolic blood pressure
  - Oxygen saturation (SpO2)
  - Respiration rate and body temperature
  - Troponin, CK-MB, and LDH levels
  - Cholesterol, HDL, LDL, and triglycerides
  - ECG ST elevation and Q-wave indicators
  - Diabetes, hypertension, smoking, obesity, inactivity, alcohol use, and stress level
- Returns a probability-based low-, medium-, or high-risk result
- Includes input validation for numerical ranges and binary fields
- Loads saved model artifacts instead of retraining the model whenever the web app starts

---

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| Flask | Web framework for the application |
| XGBoost | Machine learning classifier |
| pandas | Data processing and DataFrame creation |
| NumPy | Numerical operations |
| scikit-learn | Preprocessing and model-related utilities |
| Selenium | Browser automation / monitoring features |
| webdriver-manager | Automatically manages ChromeDriver for Selenium |
| HTML, CSS, JavaScript | Web interface |

---

## Project Structure

```text
MI-Prediction/
│
├── MI_flask/
│   ├── app.py
│   ├── mi_model.json
│   ├── preprocessors.pkl
│   ├── requirements.txt
│   │
│   ├── templates/
│   │   ├── index.html
│   │   └── live_monitor.html
│   │
│   └── static/
│       ├── normal.mp4
│       └── highrisk.mp4
│
├── mi_prediction_dataset_rich.csv
└── README.md
```

### Important Files

| File / Folder | Description |
|---|---|
| `app.py` | Main Flask application entry point |
| `mi_model.json` | Saved trained XGBoost model |
| `preprocessors.pkl` | Saved preprocessing objects, including the label encoder, imputer, and feature column order |
| `templates/index.html` | Main patient-input and prediction page |
| `templates/live_monitor.html` | Live-monitoring page |
| `static/normal.mp4` | Normal-risk visual media |
| `static/highrisk.mp4` | High-risk visual media |
| `mi_prediction_dataset_rich.csv` | Dataset used for model development/training |

---

## Prerequisites

Before running the project, install:

- Python 3.9 or later
- pip
- Google Chrome, if you use the Selenium/browser-monitoring functionality
- Git, optional but recommended for cloning the repository

Check your Python installation:

```bash
python --version
```

If `python` does not work on Windows, try:

```bash
py --version
```

---

## Installation

### 1. Clone the repository

```bash
git clone [https://github.com/YOUR-USERNAME/MI-Prediction.git](https://github.com/YOUR-USERNAME/MI-Prediction.git)
```

Move into the Flask project folder:

```bash
cd MI-Prediction/MI_flask
```

> Replace `YOUR-USERNAME` with your actual GitHub username.

### 2. Create a virtual environment

Creating a virtual environment keeps project packages separate from your system Python installation.

**Windows Command Prompt:**

```bash
python -m venv venv
venv\Scripts\activate
```

**Windows PowerShell:**

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
python3 -m venv venv
source venv/bin/activate
```

When activated, the terminal should begin with:

```text
(venv)
```

### 3. Install dependencies

First, upgrade pip:

```bash
python -m pip install --upgrade pip
```

Install the packages listed in `requirements.txt`:

```bash
python -m pip install -r requirements.txt
```

If your `requirements.txt` file is missing packages or you receive import errors, install the required packages directly:

```bash
python -m pip install flask pandas numpy scikit-learn xgboost selenium webdriver-manager joblib
```

Flask can be installed through `pip install Flask`, and XGBoost is available through `pip install xgboost`. [6][15]

---

## Run the Application

Make sure you are inside the `MI_flask` folder and that the virtual environment is active.

```bash
python app.py
```

If successful, Flask should show output similar to:

```text
Running on http://127.0.0.1:5000
```

Open the following address in your web browser:

```text
http://127.0.0.1:5000
```

To stop the server, press:

```text
Ctrl + C
```

---

## Common Errors and Fixes

### Error: `ModuleNotFoundError: No module named 'flask'`

This means Flask is not installed in the Python environment currently running the project.

Activate the virtual environment and run:

```bash
python -m pip install flask
```

Then start the application again:

```bash
python app.py
```

### Error: `can't open file 'mi_prediction.py'`

Your Flask application starts from `app.py`, not `mi_prediction.py`.

Use:

```bash
python app.py
```

Do not use:

```bash
python mi_prediction.py
```

unless you have created a Python file with that exact name.

### Error: `FileNotFoundError` for `mi_model.json` or `preprocessors.pkl`

Confirm that these files are in the same folder as `app.py`:

```text
MI_flask/
├── app.py
├── mi_model.json
└── preprocessors.pkl
```

### Error: Selenium or ChromeDriver issue

Update Selenium and webdriver-manager:

```bash
python -m pip install --upgrade selenium webdriver-manager
```

Also ensure that Google Chrome is installed and updated.

### Error: PowerShell does not allow activation

If PowerShell blocks the virtual environment activation script, use Command Prompt instead:

```bash
venv\Scripts\activate
```

Alternatively, open PowerShell as Administrator and run:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Then activate again:

```powershell
.\venv\Scripts\Activate.ps1
```

---

## Model Workflow

1. The application receives patient data from the web form.
2. It validates inputs and converts them into the required data types.
3. Gender and other relevant values are transformed using saved preprocessing objects.
4. Missing values are processed using the saved imputer.
5. The trained XGBoost model calculates the probability of MI risk.
6. The system assigns a risk category:
   - **Low Risk:** Probability below 15%
   - **Medium Risk:** Probability from 15% to below 50%
   - **High Risk:** Probability of 50% or above
7. The result is displayed in the web application.

---

## Training the Model

The model was developed using the provided clinical dataset:

```text
mi_prediction_dataset_rich.csv
```

The training pipeline included:

- Data cleaning and preprocessing
- Label encoding for gender
- Missing-value imputation using `SimpleImputer`
- Stratified train-test splitting
- Class-imbalance handling using `scale_pos_weight`
- XGBoost classification
- Evaluation using accuracy, precision, recall, F1-score, and ROC-AUC
- Saving the trained XGBoost model as `mi_model.json`
- Saving preprocessing components as `preprocessors.pkl`

---

## Future Improvements

- Add SHAP-based explanations for each prediction
- Add secure user authentication for healthcare staff
- Store prediction history in a database
- Add model calibration and uncertainty scores
- Create a dashboard for patient-risk trends
- Deploy the application using Render, Railway, AWS, or Azure
- Add Docker support for consistent deployment
- Improve clinical validation using larger and more diverse datasets

---

## Disclaimer

This project is an academic and technical demonstration of machine learning in healthcare. The prediction produced by the system is not medical advice, a medical diagnosis, or an emergency assessment. Always consult a licensed healthcare professional for medical decisions.

---

## Author

**Nisha T**

Machine Learning and Healthcare AI Project
