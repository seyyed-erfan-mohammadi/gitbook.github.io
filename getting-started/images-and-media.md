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
    visible: false
  pagination:
    visible: true
---

# Flat sensors

Flat sensors dampen the graphs due to their abnormal amplitude when plotting the signal power spectrum. Therefore, to accurately generate all graphs in the pipeline, these sensors must first be identified and excluded. At this stage, only flat sensors are detected and excluded from plotting, while the full detection of bad sensors is postponed to later steps.

```python
data.compute_psd().plot()
```

<figure><img src="../.gitbook/assets/flat.png" alt="Example of Flat sensors on PSD plot"><figcaption><p>Damped power spectrum with one flat sensor</p></figcaption></figure>

{% hint style="info" %}
If the MNE library introduces a scaling parameter for the y-axis in [PSD plots](https://mne.tools/stable/generated/mne.time_frequency.Spectrum.html#mne.time_frequency.Spectrum.plot), the MEGAP pipeline could be updated to leverage this feature.&#x20;
{% endhint %}

To identify flat sensors based on [PREP](https://www.frontiersin.org/journals/neuroinformatics/articles/10.3389/fninf.2015.00016/full), the code first detects channels containing NaN values, which indicate missing or corrupted data. It then flags channels with flat signals by evaluating two criteria: Median Absolute Deviation (MAD) and Standard Deviation (STD). Channels with MAD or STD values below a predefined threshold ([`flat_threshold = 1e-18`](https://mne.tools/stable/generated/mne.preprocessing.find_bad_channels_maxwell.html#mne.preprocessing.find_bad_channels_maxwell) `T or T/m`) are considered flat, as they exhibit no meaningful signal variation.&#x20;

```python
meg_picks_diff =  mne.pick_types(raw_meg.info, meg=True, ref_meg=False)
meg_data = raw_meg.get_data(picks=meg_picks_diff)
ch_names = [raw_meg.ch_names[i] for i in meg_picks_diff]
# Detect channels containing any NaN values
nan_channel_mask = np.isnan(np.sum(meg_data, axis=1))
nan_channels = np.array(ch_names)[nan_channel_mask]
flat_threshold=1e-18
# Detect channels with flat signals
flat_by_mad = median_abs_deviation(meg_data, axis=1) < flat_threshold
flat_by_std = np.std(meg_data, axis=1) < flat_threshold
flat_channel_mask = flat_by_mad | flat_by_std
flat_channels = np.array(ch_names)[flat_channel_mask]

flat_channel_indices=[]
flat_channel_indices.extend(meg_ch_idx[flat_channel_mask])
flat_channel_indices.extend(meg_ch_idx[nan_channel_mask])

```
