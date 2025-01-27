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

```python
# Get cHPI info; if not available, pick system-related channels
_, ch_idx, _ = mne.chpi.get_chpi_info(data.info)
if ch_idx is None:
    ch_idx = mne.pick_types(data.info, syst=True)
    ch_idx = ch_idx[0]
stim = data.get_data(picks=ch_idx)
stim_diff = np.diff(stim)

# Find the time when the cHPI turns on
sfreq = data.info['sfreq']  # Sampling frequency
start_chpi = (np.array(stim_diff).argmax() / sfreq)

print ("cHPI start time= ",start_chpi ,"sec")
print("MEG Initialization period= ",data.first_time, "sec")
```

```
Using 5 HPI coils: 293 307 314 321 328 Hz
cHPI start time=  32.206 sec
MEG Initialization period=  19.0 sec
```

{% hint style="warning" %}
These two periods depend on the MEG system and data type. Some MEG systems wait for these two periods to pass before starting to record and save valid data, while others begin recording from the very start, capturing these two periods as part of the data (e.g. [Cam-CAN dataset](https://camcan-archive.mrc-cbu.cam.ac.uk/dataaccess/)). In the latter case, the data corresponding to these initial periods is included in the file, even though it does not contain meaningful brain signals.
{% endhint %}

Eliminating these non-essential data intervals is an important preprocessing step to ensure the data is clean and ready for further analysis. While the MNE library typically handles this automatically through its built-in functions, removing these irrelevant data periods, MEGAP explicitly addresses this step for users who prefer not to rely on MNE for post-processing.
