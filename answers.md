# Task 1 — Classify and Handle PII Fields

| Field | Type of PII | Action | Justification |
|------|------------|--------|--------------|
| full_name | Direct PII | Drop / Pseudonymize | Directly identifies a person |
| email | Direct PII | Drop | Highly sensitive and unique |
| date_of_birth | Indirect PII | Mask | Use age or year only |
| zip_code | Indirect PII | Mask | Use partial ZIP |
| job_title | Indirect PII | Generalize | Avoid rare job identification |
| diagnosis_notes | Sensitive Data | Pseudonymize | Remove personal details |


# Task 2 — Ethical Issues in API Script

## Violation 1: Hardcoded API Key

Problem:
API key is exposed in code, which is insecure.

Solution:
Use environment variable instead.

```python
import os
import requests

API_URL = "https://healthstats-api.example.com/records"
API_KEY = os.getenv("API_KEY")

response = requests.get(API_URL, params={"page": 1, "key": API_KEY})




records = []

for page in range(1, 11):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    
    if response.status_code != 200:
        break
    
    data = response.json()
    
    for record in data["results"]:
        cleaned_record = {
            "age": record.get("age"),
            "region": record.get("region"),
            "diagnosis": record.get("diagnosis_notes")
        }
        records.append(cleaned_record)

save_to_database(records)





