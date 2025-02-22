---
icon: folder-arrow-down
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# Installation

MEGAP has been evaluated on Windows 10 (Intel i7-1156G7 CPU, 24GB RAM) and macOS (M1 chip, 16GB RAM). Although GPU acceleration is not required, sufficient storage is crucial: for a dataset size of N MB per subject, ensure at least N×7 storage for MEGIN systems or N×5 for CTF/BTI systems to accommodate BIDS-compliant outputs. The pipeline has been tested with Python 3.10 and supports integration with MATLAB 2023b/2024a for modules such as ZapLine-plus. To install MEGAP, follow these steps:

{% stepper %}
{% step %}
### **Clone the Repository**

```bash
git clone https://github.com/BNARGroup/MEGAP.git
cd MEGAP
```
{% endstep %}

{% step %}
### **Install Required Packages**

Install necessary packages, including MNE, MNE-BIDS, TensorFlow, MATLAB Engine, and any other dependencies:

```bash
pip install -r requirements.txt
```
{% endstep %}

{% step %}
### **Copy Required Repositories**

Clone the following repositories into the `/config` folder of MEGAP:

* [mne-matlab](https://github.com/mne-tools/mne-matlab)
* [MegNET\_2020](https://github.com/DeepLearningForPrecisionHealthLab/MegNET_2020)
* [zapline-plus](https://github.com/MariusKlug/zapline-plus)
* [eeglab](https://github.com/sccn/eeglab)
{% endstep %}

{% step %}
### **Add Data to MEGAP**

Place MEG data organized in the MEG-BIDS structure into the `result/raw` folder of MEGAP. Additionally, include the fine calibration file and a cross-talk compensation file in the `result/config` folder. Also see [BIDS Format](../basic-information/bids-format.md) for completing configuration file.&#x20;
{% endstep %}

{% step %}
### **Execute the MEGAP Script**

You can now run MEGAP using the `MEGAP.py` script and let it pre-process all of your data without any manual intervention.

MEGAP will automatically verify all necessary folders and notify you if anything is missing.

{% code fullWidth="true" %}
```python
+--------------------------------------------+--------+
|               Required Item                | Status |
+--------------------------------------------+--------+
|               Config Folder                | ✔️ OK  |
|                 Raw Folder                 | ✔️ OK  |
|           CFG Configuration File           | ✔️ OK  |
|            SSS Calibration File            | ✔️ OK  |
|              Cross-talk File               | ✔️ OK  |
| Zapline-plus-main folder from their github | ✔️ OK  |
|  eeglab-develop folder from their github   | ✔️ OK  |
| mne-matlab-master folder from their github | ✔️ OK  |
|  CNN Model folder from MegNET_2020 github  | ✔️ OK  |
+--------------------------------------------+--------+
*** All necessary folders exist for running MEGAP ***
```
{% endcode %}
{% endstep %}

{% step %}
### Outputs

Upon completion of the pipeline, the output power spectra plots can be found in the ‘PSD’ folder, and the output text files will be saved in the ‘verbose’ folder. Any warnings generated during processing are stored in the ‘warning’ folder, while the final pre-processed data, including ICA results, is located in the ‘ICA’ folder.
{% endstep %}
{% endstepper %}
