---
icon: '9'
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

# Artifacts Removal

In MEGAP, ICA is preferred over SSP because SSP requires manual specification of spatial components, which is not fully automated. SSP may also target brain signals along with artifacts if the angle between them is not ideal. ICA, especially in high SNR data like MEG, has been shown to produce better results. Additionally, ICA is more convenient due to the availability of fully automated methods, whereas SSP relies on semi-automated implementations.

The number of [ICA-picard](https://github.com/pierreablin/picard) components is automatically determined using PCA to identify the [elbow point](https://github.com/arvkevi/kneed). This approach ensures that the number of components is appropriate for the data, avoiding the limitations of using a fixed number of components.

The artifact detection process in MEGAP involves automating the identification of cardiac, eye movements and blink artifacts. Cardiac artifacts are detected by correlating the temporal representation of ICA components with the ECG channel, using a threshold of 0.5. Eye movements and blink artifacts artifacts are identified using a convolutional neural network (CNN) trained on data from 217 subjects. The data is low-pass filtered below 100 Hz and downsampled to 250 Hz before ICA is applied. The CNN detects both ocular and cardiac artifacts with [high accuracy](https://github.com/DeepLearningForPrecisionHealthLab/MegNET_2020). The detected artifact components are removed from the original data, and all artifact details, including temporal and topographical representations, are saved and plotted in the ‘ICA’ folder.

<figure><img src="../.gitbook/assets/new ICA.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The downsampling and low-pass filtering are applied only for the ICA procedure and do not affect the final data. MEGAP does not apply any low-pass filtering to the original data.
{% endhint %}

The output of this step is organized according to the [BIDS format](../basic-information/bids-format.md). To modify the artifact detection settings for ICA in MEGAP, users can adjust the configuration file under the `ica_decomposition` section. If users want to focus on a specific segment for component rejection, they can change the start and stop parameters in the configuration file for this purpose.

```json
"ica_decomposition": {
    "ecg": {
    "measure" : "correlation",
    "threshold" : 0.5,
    "reject_by_annotation" : true,
    "method" : "correlation"
    }
    "apply": {
    "start" : null,
    "stop" : null
    }
}
```

### Warning for Number of Artifact components

The number of detection of artifact components are monitored during the ICA decomposition process. If ICA fails to detect any artifact components or detects more than 5 artifact components, a warning will be triggered. This warning is stored in the [`"warning"`](../basic-information/quickstart.md#id-7.-warning-for-data-quality-monitoring) section of the results directory. The user can adjust the threshold for artifact components in the configuration file, depending on their dataset's characteristics.
