---
icon: '1'
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

# Extraneous Data

In the early stages of the MEGAP pipeline, data segments containing extraneous information are excluded to ensure the integrity of subsequent analyses. These extraneous segments are typically associated with:

1. **MEG Device Initialization**: At the start of a recording session, the MEG device performs internal checks and verifies parameters. This period does not contain meaningful brain signal data and must be removed.
2. **cHPI Coil Start-Up**: cHPI coils require time to stabilize and start accurately tracking head movements. Signals recorded during this interval are also excluded.

````
// Some code```python
# Get cHPI info; if not available, pick system-related channels
_, ch_idx, _ = mne.chpi.get_chpi_info(data.info)
if ch_idx is None:
    ch_idx = mne.pick_types(data.info, syst=True)
    ch_idx = ch_idx[0]
stim = data.get_data(picks=ch_idx)
b = np.diff(stim)

# Find the time when the cHPI turns on
sfreq = data.info['sfreq']  # Sampling frequency
start_chpi = (np.array(b).argmax() / sfreq)

print ("cHPI start time= ",start_chpi ,"sec")
print("MEG Initialization period= ",data.first_time, "sec")
```
````

```
Using 5 HPI coils: 293 307 314 321 328 Hz
cHPI start time=  22.206 sec
MEG Initialization period=  19.0 sec
```

Eliminating these non-essential data intervals is an important preprocessing step to ensure the data is clean and ready for further analysis. While the MNE library typically handles this automatically through its built-in functions, removing these irrelevant data periods, MEGAP explicitly addresses this step for users who prefer not to rely on MNE for post-processing.
