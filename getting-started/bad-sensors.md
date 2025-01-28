---
icon: '7'
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

# Bad Sensors

### MEGIN Systems

Bad sensors can significantly affect the (pre)processing steps. In MEGAP, the [find\_bad\_channels\_maxwell](https://mne.tools/stable/generated/mne.preprocessing.find_bad_channels_maxwell.html) function from the MNE library is used to identify bad sensors for MEGIN system. The parameters for this step are the same as those used in the pipeline's tSSS stage. These parameters can be found in the "bad\_channel\_maxwell" section of the configuration file.

```json
"bad_channel_maxwell": {
    "int_order": 8,
    "ext_order": 3,
    "limit" : 7,
    "min_count" : 5,
    "duration": 20.0,
    "return_scores" : true,
    "coord_frame": "head"
},
```

The locations of bad sensors and their scores are plotted and saved in the `/plot_bad_channel` folder. Additionally, the names of the bad channels are displayed on the right side of each plot.

<figure><img src="../.gitbook/assets/figure 5.png" alt=""><figcaption><p>(A) Locations of magnetometer sensors (the bad sensor is represented in red), (B) and (C) magnetometers’ scores before and after applying the threshold, respectively.</p></figcaption></figure>

### CTF/BTI Systems

For CTF and BTI devices, where Maxwell filtering isn't available, bad sensors are identified using several methods:

1. **Deviation**: Detects channels with unusually high or low overall amplitudes.
2. **Correlation**: Identifies channels that don't correlate with others.
3. **High Frequency Noise**: Detects channels with excessive high-frequency noise.
4. **RANSAC**: Finds channels that are poorly predicted by other channels.
5. **SNR (Signal-to-Noise Ratio)**: Flags channels with low SNR, which are bad in both high-frequency noise and low correlation.

These methods, from the [PyPREP](https://zenodo.org/records/10047462) on bad channel detection, help in identifying and excluding problematic channels for cleaner data analysis. The threshold for these bad channel detection methods can be adjusted in the "bad\_channel\_nonmaxwell" section of the configuration file.

```json
"bad_channel_nonmaxwell": {
    "deviation_thresh" : 5,
    "correlation_thresh": 0.4,
    "hf_thresh": 5,
    "snr_enabled" : true
},
```

{% hint style="info" %}
RANSAC is used from the [autoreject](https://autoreject.github.io/stable/index.html) library.
{% endhint %}

### Warning on Number of Bad Sensors

In the MEGAP pipeline, a warning is triggered if the number of bad sensors exceeds a defined threshold. This threshold is set in the [config\_pipeline.cfg](../basic-information/quickstart.md#id-7.-warning-for-data-quality-monitoring) file. When the number of bad sensors for each coil type (mag or grad) goes beyond the limit, the warning is saved in the `/warning` folder within the results directory.
