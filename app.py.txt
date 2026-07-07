```python
import streamlit as st
import pickle
import pandas as pd

# Load Model
with open("sleep_disorder_model.pkl", "rb") as file:
    model = pickle.load(file)

st.title("Sleep Disorder Prediction System")

st.write("Enter Sleep and Lifestyle Details")

# Inputs
gender = st.selectbox("Gender", ["Male", "Female"])

age = st.number_input("Age", min_value=18, max_value=100, value=25)

occupation = st.selectbox(
    "Occupation",
    [
        "Doctor",
        "Engineer",
        "Teacher",
        "Nurse",
        "Lawyer",
        "Salesperson",
        "Scientist",
        "Software Engineer",
        "Manager"
    ]
)

sleep_duration = st.number_input(
    "Sleep Duration (Hours)",
    min_value=1.0,
    max_value=12.0,
    value=7.0
)

quality_of_sleep = st.slider(
    "Quality of Sleep",
    1,
    10,
    5
)

physical_activity = st.number_input(
    "Physical Activity Level",
    min_value=0,
    max_value=100,
    value=50
)

stress_level = st.slider(
    "Stress Level",
    1,
    10,
    5
)

bmi_category = st.selectbox(
    "BMI Category",
    ["Normal", "Overweight", "Obese"]
)

heart_rate = st.number_input(
    "Heart Rate",
    min_value=40,
    max_value=150,
    value=70
)

daily_steps = st.number_input(
    "Daily Steps",
    min_value=0,
    max_value=30000,
    value=5000
)

# Manual Encoding
gender = 1 if gender == "Male" else 0

occupation_map = {
    "Doctor": 0,
    "Engineer": 1,
    "Teacher": 2,
    "Nurse": 3,
    "Lawyer": 4,
    "Salesperson": 5,
    "Scientist": 6,
    "Software Engineer": 7,
    "Manager": 8
}

occupation = occupation_map.get(occupation, 0)

bmi_map = {
    "Normal": 0,
    "Overweight": 1,
    "Obese": 2
}

bmi_category = bmi_map[bmi_category]

# Prediction
if st.button("Predict"):

    input_data = pd.DataFrame(
        [[
            gender,
            age,
            occupation,
            sleep_duration,
            quality_of_sleep,
            physical_activity,
            stress_level,
            bmi_category,
            heart_rate,
            daily_steps
        ]]
    )

    prediction = model.predict(input_data)[0]

    result_map = {
        0: "No Disorder",
        1: "Insomnia",
        2: "Sleep Apnea"
    }

    st.success(
        f"Predicted Sleep Disorder: {result_map.get(prediction, prediction)}"
    )
```
