---
icon: '5'
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

# Muscle Artifact

Muscle artifacts generally affect a small portion of the data and can often be excluded. However, due to their large amplitude and broad frequency range, detecting and removing them remains a challenge.

The most common approach for detecting muscle artifacts in MEG data involves annotation during pre-processing. This is accomplished using the [`annotate_muscle_zscore`](https://mne.tools/stable/generated/mne.preprocessing.annotate_muscle_zscore.html) command from the MNE library. This method works as follows:

1. **Frequency Range**: Z-scores are calculated for each magnetometer sensor within the typical frequency range of muscle artifacts (110–140 Hz).
2. **Aggregation**: These z-scores are aggregated and normalized by dividing by the square root of the number of sensors.
3. **Thresholding**: If the resulting score exceeds a threshold of 15 at any point in the data, that segment is annotated as a muscle artifact.

The parameters for muscle artifact detection in MEGAP are customizable and located in the `pipeline_config.cfg` file under the `"annotate_muscle"` section:

```json
"annotate_muscle": {
    "ch_type": "mag",
    "threshold": 15,
    "min_length_good": 1,
    "filter_freq": [110, 140]
}
```

{% hint style="info" %}
Magnetometers are specifically selected for detecting muscle artifacts because they are more sensitive to muscle activity compared to other sensor types.
{% endhint %}

#### Muscle Duration Warning

The annotation period for muscle artifacts is evaluated to ensure that the duration of the affected segments does not exceed a specified percentage of the total data. This threshold is defined in the [**"warning"**](../basic-information/quickstart.md#id-7.-warning-for-data-quality-monitoring) section of the pipeline configuration file.

If the annotated period surpasses this predefined percentage, a warning message is generated. This warning is recorded in a text file located in the **/warning** folder, alerting the user about the significant presence of muscle artifacts in the data.

{% hint style="info" %}
If you see a warning for muscle artifacts, first check the muscle z-score plot in the **plot\_muscle\_zscore** folder. Then, review the z-score threshold and warning threshold in the configuration file to understand the criteria triggering the warning.
{% endhint %}
