# Development Datasets Chamber

This chamber contains synthetic datasets aligned with vertical job postings. Each dataset is generated via `synth_data.py` and supports demo dashboards in Power BI, SQL, and Python.

---

## 📊 Available Datasets

### [Clinical Research Dataset](clinical_research.readme.md)
- **Anchor Vertical:** Medpace — Clinical Research / Life Sciences  
- **Spec File:** `spec.clinical_research.json`  
- **Purpose:** Simulate clinical trial operations, participant surveys, retention metrics, and adverse events.  
- **Constellation Fit:** Anchors **The Archive** and cross-links **The Protector**.  

---

### [Financial & Insurance Dataset](financial_insurance.readme.md)
- **Anchor Vertical:** Western & Southern — Financial Services / Insurance  
- **Spec File:** `spec.financial_insurance.json`  
- **Purpose:** Represent policies, claims, payments, and customer churn for insurance analytics.  
- **Constellation Fit:** Extends **The Bank** and bridges into **The Catalyst**.  

---

## ⚙️ How to Generate
Run the generator with the appropriate spec:

```bash
python synth_data.py --spec spec.clinical_research.json
python synth_data.py --spec spec.financial_insurance.json

