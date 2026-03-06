# Vendor_Invoice_System_Using_ML


📦 Vendor Invoice Intelligence Portal

An end-to-end machine learning system for freight cost prediction and invoice risk flagging, powered by a Streamlit dashboard.

🧠 Project Overview
This project automates two critical finance and supply chain operations using machine learning:

Freight Cost Prediction — Predicts the estimated freight cost for a vendor invoice based on the invoice dollar amount.
Invoice Manual Approval Flagging — Classifies whether a vendor invoice requires manual review or can be auto-approved, based on invoice attributes.

Both models are served through an interactive Streamlit web app (app.py) designed for internal business use.

🎯 Business Objective
1. Freight Cost Prediction
   
Objective: Predict freight cost for a vendor invoice using quantity and dollars, to improve cost forecasting, bydgeting, and vendor negotiation

Freight is a non trivial component of landed cost
Poor freight estimates distort margin and inventory planning
Automating freight estimation helps procurement teams forecast true cost before invoice arrival.
<img width="1905" height="778" alt="Screenshot 2026-03-06 165338" src="https://github.com/user-attachments/assets/79d89681-ea2d-4a6c-94fb-7d5c2b0001df" />

2. Invoice Risk Plugging

Objective: Predict whether a vendor invoice should be flagged for mannual approval based on abnormal cost, freight, or delivery patterns in order to reduce financial risk, improve operational efficiency and prioritize human reviewwhere it adds most value.

Manual invoice review is time consuming and does not scale with transaction volume
Abnormal freight charges, pricing deviations, or deliviries delays often indicate errors, disputes or compliances risks.
An automated flagging system enables finance teams to focus attention on high-risk invoices while allowing risk invoices to processed automatically
<img width="1890" height="773" alt="Screenshot 2026-03-06 170003" src="https://github.com/user-attachments/assets/75e7e8f7-07bc-4ebd-8592-6e252b596b19" />


📁 Project Structure

Machine_Learning_Project/
│
├── data/
│   └── inventory.db                   # SQLite database containing vendor invoice data
│
├── freight_cost_prediction/
│   ├── models/                        # Saved trained model (.pkl)
│   ├── data_preprocessing.py          # Load, feature prep, and train/test split
│   ├── model_evaluation.py            # Train & evaluate Linear, Decision Tree, Random Forest
│   └── train.py                       # Main training pipeline; saves best model
│
├── invoice_flagging/
│   ├── models/                        # Saved trained model (.pkl)
│   ├── data_preprocessing.py          # Preprocessing for invoice flagging
│   ├── modeling_evaluation.py         # Model training & evaluation
│   └── train.py                       # Main training pipeline for flagging model
│
├── inference/
│   ├── predict_freight.py             # Inference script for freight cost prediction
│   └── predict_invoice_flag.py        # Inference script for invoice approval flag
│
├── notebooks/
│   ├── Predicting Freight Cost.ipynb  # EDA & experimentation for freight prediction
│   └── Invoice_Flagging.ipynb         # EDA & experimentation for invoice flagging
│
├── app.py                             # Streamlit web application
└── README.md


🔬 Machine Learning Pipeline
1. Freight Cost Prediction
| Step            | Details                                                             |
| --------------- | ------------------------------------------------------------------- |
| Data Source     | vendor_invoice table from inventory.db                              |
| Feature         | Dollars (Invoice dollar amount)                                     |
| Target          | Freight (Freight cost)                                              |
| Models Trained  | Linear Regression, Decision Tree Regressor, Random Forest Regressor |
| Model Selection | Best model selected by lowest MAE                                   |
| Saved Model     | freight_cost_prediction/models/predict_freight_model.pkl            |

2. Invoice Manual Approval Flagging
| Step        | Details                                                                             |
| ----------- | ----------------------------------------------------------------------------------- |
| Data Source | vendor_invoice table from inventory.db                                              |
| Features    | invoice_quantity, freight, invoice_dollars, total_item_quantity, total_item_dollars |
| Target      | Binary flag — 1 = Requires Manual Approval, 0 = Safe for Auto Approval              |
| Saved Model | invoice_flagging/models/predict_flag_invoice.pkl                                    |

🚀 Getting Started
Prerequisites
Make sure you have the following installed:
pip install pandas scikit-learn joblib streamlit plotly

1. Train the Models
Freight Cost Model:
cd freight_cost_prediction
python train.py

Invoice Flagging Model:
cd invoice_flagging
python train.py


Both scripts will:

Load data from data/inventory.db
Train multiple models
Evaluate performance
Save the best model to the respective models/ directory

2. Run the Streamlit App
From the root of the project:
streamlit run app.py


🖥️ Streamlit App — Features
The app (app.py) provides a user-friendly interface with two prediction modules, selectable from the sidebar:
🚚 Freight Cost Prediction

Input: Invoice Dollars
Output: Estimated Freight Cost (in USD)

🚨 Invoice Manual Approval Flag

Input: Invoice Quantity, Freight Cost, Invoice Dollars, Total Item Quantity, Total Item Dollars
Output: ✅ Safe for Auto Approval or 🚨 Requires Manual Approval


📊 Model Evaluation Output

Linear Regression performance:
  MAE  : 12.45
  RMSE : 18.32
  R2   : 87.50%

Decision Tree Regression performance:
  MAE  : 10.11
  RMSE : 15.60
  R2   : 90.20%

Random Forest Regression performance:
  MAE  : 9.87
  RMSE : 14.90
  R2   : 91.45%

Best model saved: Random Forest Regression
Model Path: .../models/predict_freight_model.pkl

🗂️ Key Files Reference
| File                    | Purpose                                                      |
| ----------------------- | ------------------------------------------------------------ |
| data_preprocessing.py   | Loads SQLite data, selects features, splits into train/test  |
| model_evaluation.py     | Trains and evaluates regression models, returns metrics dict |
| train.py                | Orchestrates training, selects best model, saves with joblib |
| predict_freight.py      | Loads saved model, returns DataFrame with Predicted_Freight  |
| predict_invoice_flag.py | Loads saved model, returns DataFrame with Predicted_flag     |
| app.py                  | Streamlit UI connecting inference scripts to end users       |


💼 Business Impact

📈 Improved cost forecasting — Accurate freight estimates support better budget planning
🗂️ Reduced invoice fraud & anomalies — Automated flagging reduces manual review overhead
⚙️ Faster finance operations — Auto-approval for low-risk invoices speeds up the payment cycle


📌 Notes

The database path is resolved dynamically using Path(__file__).resolve().parent.parent in train.py, so the scripts work regardless of where they are run from.
Models are serialized using joblib for efficient loading during inference.
Inference scripts (predict_freight.py, predict_invoice_flag.py) can also be run standalone with sample data for quick testing.



