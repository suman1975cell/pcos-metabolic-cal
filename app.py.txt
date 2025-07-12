import streamlit as st
import numpy as np

# Page config
st.set_page_config(page_title="PCOS Metabolic Risk Calculator", layout="centered")

# Title
st.title("🧮 PCOS Metabolic Risk Calculator")
st.markdown("""
This tool predicts the **risk of metabolic syndrome** in women with PCOS using clinical parameters like:
- Body Mass Index (BMI)
- Waist Circumference
- Triglycerides
- HDL Cholesterol
- Fasting Blood Sugar (FBS)
""")

# Input fields
bmi = st.number_input("BMI (kg/m²)", min_value=15.0, max_value=50.0, value=28.0)
waist = st.number_input("Waist Circumference (cm)", min_value=60.0, max_value=120.0, value=85.0)
triglycerides = st.number_input("Triglycerides (mg/dL)", min_value=50.0, max_value=400.0, value=150.0)
hdl = st.number_input("HDL (mg/dL)", min_value=20.0, max_value=100.0, value=45.0)
fbs = st.number_input("Fasting Blood Sugar (mg/dL)", min_value=60.0, max_value=200.0, value=95.0)

# Logistic regression coefficients (from simulated model)
intercept = -46.8567
coef_bmi = 0.2386
coef_waist = 0.3346
coef_tg = 0.0239
coef_hdl = 0.0048
coef_fbs = -0.0150

# Calculate logit and probability
logit = (
    intercept +
    coef_bmi * bmi +
    coef_waist * waist +
    coef_tg * triglycerides +
    coef_hdl * hdl +
    coef_fbs * fbs
)
prob = 1 / (1 + np.exp(-logit))

# Output
st.subheader("📊 Metabolic Risk Estimate")
if prob < 0.33:
    st.success(f"Low Risk (Probability: {prob:.2f})")
elif prob < 0.66:
    st.warning(f"Moderate Risk (Probability: {prob:.2f})")
else:
    st.error(f"High Risk (Probability: {prob:.2f})")

st.markdown("---")
st.caption("🔬 This tool is based on a simulated logistic regression model. For research and educational use only. Not a substitute for medical advice.")





