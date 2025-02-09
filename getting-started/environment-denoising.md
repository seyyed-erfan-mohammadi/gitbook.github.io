---
icon: '8'
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

## MEGIN Systems

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

In MEGAP, it is possible to switch from tSSS to SSS if preferred. This is particularly useful when tSSS may excessively reduce the brain signal, such as during excessive head movements or if metal is near the sensors. Users can disable tSSS and choose standard SSS by setting `st_duration: null` in the configuration file.

## CTF Systems

In MEGAP, CTF [third-order gradient compensation](https://mne.tools/stable/generated/mne.io.Raw.html#mne.io.Raw.apply_gradient_compensation) is applied as an alternative to tSSS, which is not evaluated for CTF data. This approach helps reduce environmental noise while preserving the brain signal. [Before applying gradient compensation](https://www.sciencedirect.com/science/article/pii/S1053811922001768),  bad channels are interpolated using the [minimum norm](https://mne.tools/stable/generated/mne.io.Raw.html#mne.io.Raw.interpolate_bads) method to ensure data quality. This step is crucial because noise suppression algorithms, which rely on linear transformations, can spread artifacts from malfunctioning or flat sensors across the sensor array.

## BTI Systems

Digital balancing weights are used to reduce environmental noise in BTI data by subtracting weighted signals from distant reference coils. For 248-channel systems, these weights are typically computed using linear regression via BTi/4D software, [FieldTrip](https://www.fieldtriptoolbox.org/getting_started/meg/bti/), or [MNE-HCP](https://mne.tools/mne-hcp/auto_tutorials/plot_reference_correction.html#sphx-glr-auto-tutorials-plot-reference-correction-py). While MNE-HCP has not been updated in years and is no longer compatible with the current MNE, we have adapted its code into our pipeline to enable effective environmental denoising for BTI data. Similar to the CTF data processing, bad channels are interpolated first using the minimum norm method before applying linear regression for environmental noise reduction in BTI data.

```python
meg_picks = mne.pick_types(data.info, meg=True, ref_meg=False)
ref_picks = mne.pick_types(data.info, meg=False, ref_meg=True)

# Extract reference and MEG data
ref_data = data.get_data(picks=ref_picks)
meg_data = data.get_data(picks=meg_picks)

decim_fit = 100

estimator = Pipeline([('scaler', StandardScaler()), ('estimator', LinearRegression())])

# Prepare data for fitting
X = ref_data[:, ::decim_fit].T
Y = meg_data[:, ::decim_fit].T

# Fit and predict
Y_pred = estimator.fit(X, Y).predict(ref_data.T)

# Subtract predicted values
data._data[meg_picks] -= Y_pred.T
```

First, it separates the data into MEG sensors and reference sensors, where the reference sensors primarily capture environmental noise. A regression model is trained to learn the relationship between the noise in the reference sensors and the signals in the MEG sensors, using a subset of the data for efficiency. Once trained, the model predicts the noise contribution from the reference sensors to the MEG signals. This predicted noise is then subtracted from the MEG data, effectively reducing the environmental interference and leaving cleaner signals for analysis.



