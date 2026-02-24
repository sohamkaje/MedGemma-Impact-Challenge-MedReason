# 🩺 MedReason — Structured Diagnostic Reasoning with MedGemma

> An interactive Google Colab notebook for medical education that uses **MedGemma-4b-it** (Google HAI-DEF) to reason through clinical cases and generate structured differential diagnoses.

---

## Overview

MedReason is a sandbox tool that helps medical students practice structured clinical reasoning. You input a patient case — symptoms, vitals, history, labs — and MedGemma reasons through it step-by-step, returning a structured analysis with ranked diagnoses, red flags, and suggested next tests.

**This is strictly an educational tool. It does not constitute clinical advice, diagnosis, or treatment recommendations.**

---

## Example Output

```
══════════════════════════════════════════════════════════════
  🩺  MEDREASON — STRUCTURED DIAGNOSTIC ANALYSIS
══════════════════════════════════════════════════════════════

📋  CLINICAL SUMMARY
──────────────────────────────────────────────────────────────
58-year-old male with sudden crushing chest pain radiating to
the left arm, diaphoresis, and nausea. Multiple cardiac risk
factors present including diabetes, hypertension, and smoking.

🚨  RED FLAGS & RISK INDICATORS
──────────────────────────────────────────────────────────────
  • Crushing chest pain with radiation to left arm
  • Diaphoresis and nausea — autonomic symptoms
  • Elevated BP and tachycardia on presentation
  • Multiple major cardiac risk factors

🔍  RANKED DIFFERENTIAL DIAGNOSES
──────────────────────────────────────────────────────────────
  #1 🔴 [High likelihood]  STEMI / Acute Myocardial Infarction
      ↳ Classic presentation with risk factor burden...

  #2 🟡 [Medium likelihood]  Unstable Angina / NSTEMI
      ↳ Cannot exclude without troponin result...

  #3 🟢 [Low likelihood]  Aortic Dissection
      ↳ Must rule out given severity of presentation...

🧪  SUGGESTED NEXT DIAGNOSTIC STEPS
──────────────────────────────────────────────────────────────
  • 12-lead ECG (stat) — identify ST elevation or LBBB
  • Troponin I/T (stat + repeat at 3h) — confirm myocardial injury
  • Chest X-ray — assess mediastinum, pulmonary edema

✅  REASONING CONFIDENCE: High
──────────────────────────────────────────────────────────────
Strong clinical picture with multiple supporting features.
```

---

## Quickstart

### Step 1 — Get MedGemma access
1. Create a free account at [huggingface.co](https://huggingface.co)
2. Visit [huggingface.co/google/medgemma-4b-it](https://huggingface.co/google/medgemma-4b-it)
3. Click **"Access repository"** and agree to the HAI-DEF Terms of Use (approval is instant)
4. Generate a **Read** token at [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens)

### Step 2 — Add your token to Colab Secrets
1. Open the notebook in Google Colab
2. Click the 🔑 **key icon** in the left sidebar
3. Add a new secret: **Name** = `HF_TOKEN`, **Value** = your `hf_...` token
4. Toggle **"Notebook access"** ON

### Step 3 — Enable GPU
- Runtime → Change runtime type → **T4 GPU** → Save

### Step 4 — Run the notebook
Run all cells in order:
1. **Step 1** — Install dependencies
2. **Step 2** — Load token from Colab Secrets
3. **Step 3** — Load MedGemma (~2 min on first run, downloads model weights)
4. **Cases 1–3** — Run the provided test cases
5. **Try Your Own Case** — Input your own patient presentation

---

## Usage

```python
result = analyze(
    age      = "45",
    sex      = "Female",
    symptoms = "Progressive fatigue, weight gain, cold intolerance, dry skin",
    history  = "No significant PMH. Maternal aunt has Hashimoto's thyroiditis.",
    vitals   = "BP 105/68, HR 58, Temp 36.1°C",   # optional
    labs     = "CBC normal, TSH pending"            # optional
)
```

### Parameters

| Parameter  | Required | Description |
|------------|----------|-------------|
| `age`      | ✅ | Patient age (string) |
| `sex`      | ✅ | `"Male"`, `"Female"`, or `"Other"` |
| `symptoms` | ✅ | Free-text description of presenting complaints |
| `history`  | ✅ | Past medical, family, and social history |
| `vitals`   | Optional | BP, HR, RR, Temp, SpO2 |
| `labs`     | Optional | Any available lab or imaging results |

---

## Structured Output Fields

Every analysis returns the following fields:

| Field | Description |
|-------|-------------|
| `summary` | 2–3 sentence clinical summary |
| `risk_flags` | List of red flags and risk indicators |
| `differential_ranked` | Ranked list of diagnoses with justification and likelihood |
| `next_tests` | Suggested diagnostic tests with rationale |
| `confidence_level` | `High`, `Medium`, or `Low` |
| `confidence_rationale` | Explanation of the confidence rating |

---

## Included Test Cases

| Case | Presentation | Key Teaching Point |
|------|-------------|-------------------|
| Case 1 | 58M — Crushing chest pain, diaphoresis | High-risk cardiac emergency |
| Case 2 | 24F — Fatigue, weight gain, cold intolerance | Endocrine workup (hypothyroidism) |
| Case 3 | 7M — Fever, non-blanching rash, neck stiffness | Pediatric emergency (meningococcemia) |

---

## Requirements

- Google Colab with **T4 GPU** (free tier works)
- Hugging Face account with MedGemma access approved
- Python packages (auto-installed in the notebook):
  - `transformers`
  - `accelerate`
  - `bitsandbytes`

> The model runs in **4-bit quantization** to fit within Colab's free-tier GPU memory (15GB).

---

## Model

This project uses **[google/medgemma-4b-it](https://huggingface.co/google/medgemma-4b-it)** — a 4-billion parameter instruction-tuned medical language model from Google's Health AI Developer Foundations (HAI-DEF) collection. MedGemma is pre-trained on curated medical datasets including biomedical literature, clinical notes, and medical imaging reports.

Use of this model is subject to the [HAI-DEF Terms of Use](https://developers.google.com/health-ai-developer-foundations/terms).

---

## Disclaimer

This tool is for **educational and research purposes only**. It is not a medical device, does not provide clinical advice, and should never be used to make real patient care decisions. Always consult a qualified healthcare professional.

---

## License

MIT License — see `LICENSE` for details.

*Built for the [MedGemma Impact Challenge](https://www.kaggle.com/competitions/medgemma-impact-challenge) · Google HAI-DEF · 2026*
