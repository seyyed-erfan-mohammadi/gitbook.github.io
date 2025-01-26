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

# Installing

{% stepper %}
{% step %}
### **Clone the Repository**

```
git clone https://github.com/BNARGroup/MEGAP.git
cd MEGAP
```
{% endstep %}

{% step %}
### **Install Required Packages**

Install necessary packages, including MNE, MNE-BIDS, TensorFlow, MATLAB Engine, and any other dependencies:

```
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

Place MEG data organized in the MEG-BIDS structure into the `result/raw` folder of MEGAP. Additionally, include the fine calibration file and a cross-talk compensation file in the `result/config` folder.
{% endstep %}

{% step %}
### **Execute the MEGAP Script**

You can now run MEGAP using the `MEGAP.py` script and let it pre-process all of your data without any manual intervention.

MEGAP will automatically verify all necessary folders and notify you if anything is missing.

{% code fullWidth="true" %}
```
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

Upon completion of the pipeline, the output power spectra plots can be located in the ‘PSD’ folders, while the output text files will be found in the ‘verbose’ folder.&#x20;
{% endstep %}
{% endstepper %}
