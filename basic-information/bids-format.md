---
icon: memo-circle-info
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# BIDS Format

If you're unfamiliar with the [BIDS (Brain Imaging Data Structure) standard](https://www.nature.com/articles/sdata2018110), refer to the [MNE-BIDS documentation](https://mne.tools/mne-bids/stable/index.html) for more details on its structure and usage.

The configuration file for the MEGAP pipeline is written in JSON format and consists of two key sections: **`BIDS_structure_read`** and **`BIDS_structure_write`**. These sections allow users to specify parameters for reading from and writing to BIDS-compliant datasets.

### **`BIDS_structure_read`**

This section specifies parameters required for reading data from the BIDS dataset:

```json
"BIDS_structure_read": {
    "session": "rest",
    "task": "rest",
    "suffix": "meg",
    "datatype": "meg",
    "extension": ".fif"
}
```

* **`session`**: Defines the session name (e.g., "rest" for resting-state data).
* **`task`**: Describes the task performed during data acquisition (e.g., "rest").
* **`suffix`**: Indicates the file suffix, typically identifying the type of data (e.g., "meg" for MEG files).
* **`datatype`**: Specifies the data type (e.g., "meg").
* **`extension`**: Indicates the file extension (e.g., ".fif" for MEG data in FIFF format).

### **`BIDS_structure_write`**

This section defines parameters for organizing and saving preprocessed outputs in the BIDS format:

```json
"BIDS_structure_write": {
    "session": "rest",
    "task": "rest",
    "suffix": "meg",
    "datatype": "meg",
    "extension": ".fif"
}
```

* The same parameters are used as in the **`BIDS_structure_read`** section, ensuring consistency between the input and output data structures.

To ensure the MEGAP pipeline runs correctly, users must first complete the **`BIDS_structure_read`** and **`BIDS_structure_write`** sections in the configuration file. If any of the configuration fields are not required for your specific dataset, you can set them to `null`.
