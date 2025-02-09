---
icon: file-circle-info
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

# Folder Structure

The MEGAP pipeline organizes its outputs within a structured directory system to streamline processing, analysis, and storage of MEG data. The top-level directory, named `result`, serves as the root for all pipeline-generated files and subfolders. Below is a detailed explanation of the structure and purpose of each folder:

```
/result/ 
└─── config/
    ├── eeglab-develop/
    ├── mne-matlab-master/
    ├── zapline-plus-main/
    ├── MegNET_2020-main/
    ├── Pipeline_config.cfg
    ├── Ct_sparse.fif
    └── sss_cal.dat
│
├── raw/
├── data/
├── PSD_data/
├── flat_channel/
├── head_position/
├── plot_head_pos/
├── filter_chpi/
├── PSD_filter_chpi/
├── zapline_plus/
├── PSD_zapline/
├── multitaper_removal/
├── PSD_multitaper_removal/
├── annotate_muscle/
├── plot_muscle_zscore/
├── bad_channel/
├── plot_bad_channel/
    ├── location/ 
    ├── score_mag/
    └── score_grad/
│
├── OTP/
├── PSD_OTP/
├── environment_denoising/
├── PSD_environment_denoising/
├── ICA/
├── PSD_ICA/
├── plot_ICA/
├── signal_before_ica/ 
    ├── Original/
    └── Zoomed/
│
├── signal_after_ica/
    ├── Original/
    └── Zoomed/
│
├── preprocessed_BIDS_output/
└── verbose/
└── warning/
```

<details>

<summary><strong>Configuration Files: <code>config/</code></strong></summary>

The `config` folder contains essential files, repositories, and configuration parameters required to run the pipeline. Its contents include:

* **`eeglab-develop/`**: [EEGlab](https://github.com/sccn/eeglab) repository from GitHub, essential for running `Zapline_plus`.
* **`mne-matlab-master/`**:[ MNE-MATLAB](https://github.com/mne-tools/mne-matlab) repository from GitHub for interfacing MEG data with MATLAB.
* **`zapline-plus-main/`**: [Zapline-plus](https://github.com/MariusKlug/zapline-plus) repository from GitHub for filtering line noise.
* **`MegNET_2020-main/`**: Pretrained [MEGNet\_2020](https://github.com/DeepLearningForPrecisionHealthLab/MegNET_2020) model for automatic artifact detection in ICA.
* **`Pipeline_config.cfg`**: The configuration file specifying parameters for running the pipeline.
* **`Ct_sparse.fif` & `sss_cal.dat`**: Calibration and crosstalk compensation files required for MEGIN system .

</details>

<details>

<summary><strong>Raw Data: <code>raw/</code></strong></summary>

Contains raw MEG signals organized in the BIDS format, ensuring compatibility with standardized neuroimaging pipelines.

</details>

<details>

<summary><strong>Intermediate Data</strong></summary>

* **`data/`**: Stores output from the first pipeline step, with extraneous data removed.
* **`PSD_data/`**: Power spectral density (PSD) of the cleaned data.
* **`flat_channel/`**: Contains information about flat channels identified during preprocessing.
* **`head_position/`**: Tracks head movement data across recordings.
* **`plot_head_pos/`**: Visualizations of head position data, providing insights into participant stability.
* **`filter_chpi/`**: Contains filtered continuous head positioning indicator (cHPI) data.
* **`PSD_filter_chpi/`**: PSD results post-cHPI filtering.

</details>

<details>

<summary> <strong>Line Noise Filtering</strong></summary>

* **`zapline_plus/`**: Contains data processed with Zapline-plus in MATLAB for line noise removal.
* **`PSD_zapline/`**: PSD of data after Zapline-plus filtering.
* **`multitaper_removal/`**: Data processed using a regression-based multitaper method for line noise removal.
* **`PSD_multitaper_removal/`**: PSD of data after multitaper noise removal.

</details>

<details>

<summary><strong>Detection and Annotation</strong></summary>

* **`annotate_muscle/`**: Stores muscle artifact annotations generated during processing.
* **`plot_muscle_zscore/`**: Z-score plots of magnetometer data for muscle artifact detection.
* **`bad_channel/`**: Contains information about identified bad channels.
* **`plot_bad_channel/`**: Visual representations of bad channels, including:
  * **`location/`**: Visuals of bad channel locations.
  * **`score_mag/`**: Magnetometer scores.
  * **`score_grad/`**: Gradiometer scores.

</details>

<details>

<summary><strong>OTP Processing: <code>OTP/</code></strong></summary>

* Output from the OTP step, with PSD results stored in `PSD_OTP/`.

</details>

<details>

<summary> Environment Denoising<strong>:</strong></summary>

* **`environment_denoising/`**: Results from the environment denoising process to reduce noise.
* **`PSD_environment_denoising/`**: PSD results of environment denoising.

</details>

<details>

<summary><strong>ICA: <code>ICA/</code></strong></summary>

* **`ICA/`**: Final Pre-processed Data in BIDS Format
* **`PSD_ICA/`**: PSD of data after ICA.
* **`plot_ICA/`**: ICA component plots for each participant, organized by participant ID.

</details>

<details>

<summary><strong>Signal Plots</strong></summary>

* **`signal_before_ica/`**: Data before ICA, stored in two formats:
  * **`Original/`**: Full-scale signal plots.
  * **`Zoomed/`**: Zoomed-in versions for detailed inspection.
* **`signal_after_ica/`**: Preprocessed data after ICA, with similar subfolders for original and zoomed signals.

</details>

<details>

<summary><strong>Output Logs</strong></summary>

* **`verbose/`**: Detailed log files generated during the pipeline's execution. These provide a record of processing steps and debug information for troubleshooting.
* **`warning/`**:Contains warning logs generated during the pipeline’s execution with the help of [user threshold](quickstart.md#id-7.-warning-for-data-quality-monitoring). Each file is named after the corresponding subject (e.g., `sub_01.txt)`.

</details>
