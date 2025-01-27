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

# Environment Denoising

## MEGIN

The temporal Signal Space Separation (tSSS) algorithm, implemented in the [MNE library](https://mne.tools/stable/generated/mne.preprocessing.maxwell_filter.html), is crucial for isolating brain signals from external sources in MEG data. This algorithm relies on specific information about the arrangement and orientation of MEG sensors, typically provided by the device manufacturer for each model. This information is stored in two files, commonly named the fine calibration file and the crosstalk compensation file, which should be placed in the MEGAP config folder.

To effectively compensate for head movements, the tSSS algorithm requires the output from the ‘Head Movement’ stage of the MEGAP, i.e., detected head movements. The algorithm accepts various parameters as inputs. Notably, the parameters int\_order and ext\_order are crucial inputs determined through data simulation studies, where values of 8 and 3 were found suitable. Specific parameters such as `st_duration=20` and `st_correlation=0.999` have been recommended for integrating tSSS with OTP.

```json
"apply_environment_denoising": {
    "switch":true
},
"filter_maxwell": {
    "origin": "auto",
    "int_order": 8,
    "ext_order": 3,
    "st_duration": 20.0,
    "st_correlation": 0.9999,
    "coord_frame": "head",
    "destination": null,
    "regularize": "in",
    "ignore_ref": false,
    "bad_condition": "error",
    "st_fixed": true,
    "st_only": false,
    "mag_scale": "auto"
},
```

{% hint style="info" %}
It is possible to turn off the environment denoising algorithm in the configuration file.
{% endhint %}

In MEGAP, it is possible to switch from tSSS to SSS if preferred. This is particularly useful when tSSS may excessively reduce the brain signal, such as during excessive head movements or if metal is near the sensors. Users can disable tSSS and choose standard SSS by setting st\_`duration: null` in the configuration file.

## CTF
