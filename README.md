# Rural-Water-Quality-Assessment-Programmatic-Logic
This repository contains a lightweight Python tool designed to automate water quality evaluations and streamline intervention workflows for public health surveillance.

Overview

The notebook translates official regulatory guidelines—specifically the Kenya Water Service Regulatory Board (WASREB) standards and the E. coli / Risk-Of-Contamination (ROC) risk matrix—into clean, executable Python functions. It helps field scientists and water safety managers quickly determine whether a water source is safe or requires immediate intervention.  

Key Features

General Safety Screening (evaluate_water_safety): Automatically checks microbiological parameters ($E. coli$ and coliform presence), chemical limits (ammonia $\le 0.5 \text{ mg/l}$), and acceptable pH bounds ($6.5 - 8.5$).  Intervention Priority Mapping (assess_action_priority): Combines microbial risk classes ($A$ through $E$) with field Risk-Of-Contamination (ROC) scores ($0$ to $9$) to categorize urgency levels from "No action" to "Urgent action".  Automated Reporting: Generates concise assessment summaries for individual water samples. 

Code Implementation

### 1. Evaluating General Water Safety
```python

def evaluate_water_safety(coliforms_present, e_coli_present, ph_value, ammonia_value):
    """
    Evaluates whether a water source requires action based on general safety standards:
    - Coliforms: Must be absent (False)
    - E. coli: Must be absent (False)
    - pH: Must be between 6.5 and 8.5 inclusive
    - Ammonia: Must not exceed 0.5 mg/l
    """
    if coliforms_present or e_coli_present or ph_value < 6.5 or ph_value > 8.5 or ammonia_value > 0.5:
        return "Action required"
    else:
        return "No action required"
```

### 2. Assessing Action Priority
```python

def assess_action_priority(col_class, roc_score):
    """
    Determines action priority tiers based on E. coli class and ROC score
    - No action
    - Low action priority
    - Higher action priority
    - Urgent action
    """
    col_class = col_class.upper()

    if col_class == 'A' and roc_score == 0:
        return "No action"
    elif (col_class == 'A' or col_class == 'B') and roc_score < 4:
        return "Low action priority"
    elif col_class in ['D', 'E'] or roc_score >= 7:
        return "Urgent action"
    else:
        return "Higher action priority"
```

---

## Example Usage

Here is how you can run a complete assessment report for a sample (e.g., Sample #104)[cite: 1]:

```python

# Sample #104 Data
sample_coliforms = True
sample_e_coli = False
sample_ph = 7.0
sample_ammonia = 0.3
sample_class = 'B'
sample_roc = 2

# Run Evaluations

safety_result = evaluate_water_safety(sample_coliforms, sample_e_coli, sample_ph, sample_ammonia)
priority_result = assess_action_priority(sample_class, sample_roc)

# Print Final Report

print("--- Water Source Assessment Report ---")
print(f"Safety Status: {safety_result}")
print(f"Intervention Priority: {priority_result}")
```

