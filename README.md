# AVES Reality – Customer Success Case Study: OpenDRIVE Import Fix

## 📝 Scenario Overview
A B2B customer encountered a critical "Segmentation Fault" crash in the AVES Launcher while importing an OpenDRIVE (.xodr) file. This repository contains the diagnostic tools and the automated patcher used to unblock the customer for their demo.

---

## 🛠 1. Problem Summary

**Symptom:**  
Launcher crash (Segmentation Fault) during RoadMeshBuilder execution.

**Root Cause:**  
The logs identified a `[WARN] Road id missing value`. The OpenDRIVE standard requires a unique ID for every road. The C++ builder likely attempted to access a null pointer or empty string, causing the crash.

**Impact:**  
Critical blocker for the scheduled customer demo.

---

## ⚙️ 2. Environment Setup

This project uses zero external dependencies for the core logic to ensure maximum compatibility. You only need a standard Python environment with Jupyter installed.

### Option A: Using Conda (Recommended)

```bash
# Create a new environment
conda create -n aves-study python=3.9

# Activate the environment
conda activate aves-study

# Install Jupyter
conda install jupyter
````

### Option B: Using Pip

```bash
# Create a virtual environment
python -m venv aves_env

# Activate it

## On Windows:
aves_env\Scripts\activate

## On macOS/Linux:
source aves_env/bin/activate

# Install Jupyter
pip install jupyter
```

### Requirements

* Python 3.7+
* `xml.etree.ElementTree` (Included in Python Standard Library)
* `os` (Included in Python Standard Library)

---

## 🚀 3. How to Run

**Prepare the Data:**
Place the original `road.xodr` file provided by the customer in the root directory of this project.

**Launch Jupyter:**

```bash
jupyter notebook
```

**Execute the Fix:**

* Open `AVES_Solution_Notebook.ipynb`
* Run all cells (`Cell > Run All`)

**Verify:**

* The script will generate `road_patched.xodr`
* A text-based report will appear in the notebook showing which IDs were fixed
* Use [https://odrviewer.io](https://odrviewer.io) to upload the patched file and verify the geometry

---

## 🔍 4. Support Strategy

### Debugging Approach

* **Log Tracing:** Correlated the Segmentation Fault with the "Road id missing" warning
* **Schema Validation:** Scanned the XML structure to find empty attributes in the `<road>` elements
* **Quick Fix:** Developed a non-destructive script to inject IDs without altering the road geometry

### Customer Communication

* **Immediate Value:**
  "We have identified a schema inconsistency in the file and have provided a patched version below to ensure your demo runs smoothly."

* **Proactive Discovery:**
  "To help our engineering team, could you let us know which tool generated this file? We want to ensure our next update handles this specific output automatically."

### Long-Term Fix (Engineering)

* **Graceful Failure:**
  Update `RoadMeshBuilder.cpp` to validate IDs before processing, throwing a caught exception instead of a segfault

* **Pre-import Validation:**
  Add a lightweight "Sanity Check" step in the AVES Launcher UI to warn users of missing mandatory OpenDRIVE attributes

---

## 👤 Author

**Name:** Diego Alarcón
**Role:** Candidate for Customer Success Engineer
