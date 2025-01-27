---
icon: house-blank
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

# Core Features

MEGAP is designed to address the challenges of large-scale MEG data pre-processing through a suite of user-centric features that prioritize automation, standardization, and transparency. Below, we detail the pillars of its design:

<details>

<summary>1. Automated Execution</summary>

MEGAP runs seamlessly from start to finish without requiring manual intervention, minimizing human error and accelerating workflows. This ensures consistency across datasets, even in studies with hundreds of participants.

MEGAP processes data through a structured sequence of pre-defined steps (e.g., noise removal, artifact correction), applying each step to all subjects in a dataset before advancing to the next stage. The core of the MEGAP  is a Python script (`MEGAP.py`) that orchestrates the entire pre-processing workflow. This script imports all necessary modules and executes each step sequentially, from data initialization to final artifact removal. Below is a snippet of the script showcasing its structure and functionality.

```python
subject_ids = list_files()

for subject_id in subject_ids:

    subject_id = [subject_id]  

    # 1. Crop the data segments containing extraneous data for each subject ID.
    chpi_crop(subject_id)

    # 2. Detect and mark flat channels in the MEG data for each subject ID.
    flat_channel(subject_id)

    # 3. Plot the power spectral density (PSD) of the MEG data for each subject ID, with the label "data".
    plot_psd(subject_id, label="data")

    # # # 4. Estimate the head position for each subject ID.
    head_position(subject_id)

    # 5. Plot the estimated head position for each subject ID.
    plot_head_position(subject_id)

    # 6. Filter the continuous head position indicator (cHPI) signals in the MEG data for each subject ID.
    filter_chpi(subject_id)
    .
    .
    .
```

</details>

<details>

<summary>2. <strong>Configuration Files</strong></summary>

While defaults are optimized for most datasets, MEGAP allows users to adjust parameters via customizable configuration files. This flexibility ensures adaptability to unique experimental needs without compromising the pipeline’s structure.

The `config` folder in MEGAP houses all files required to customize and control the pipeline’s behavior. Located at `result/config/`, it includes the primary configuration file (`pipeline_config.cfg`), which defines parameters for each processing step. By centralizing settings and resources, the `config` folder ensures reproducibility, simplifies adjustments, and maintains a clean separation between raw data, outputs, and pipeline logic.

{% code overflow="wrap" %}
```json
"filter_chpi": {
    "include_line": false,
    "t_step": 0.001,
    "t_window": 0.2,
    "ext_order": 1,
    "allow_line_only": false
},
"head_position": {
    "amplitudes": {
        "t_step_min": 0.25,
        "t_window": "auto",
        "ext_order": 1,
        "tmin": 0,
        "tmax": null
    },
    .
    .
    .
```
{% endcode %}

</details>

<details>

<summary>3. <strong>Organized Output</strong></summary>

The MEGAP pipeline requires input data to be organized in the Brain Imaging Data Structure (BIDS) format, located in the `/raw/` folder, ensuring consistency and compatibility with neuroimaging standards. All outputs are systematically organized into participant-specific folders within the `result/` directory, following the MEG-BIDS extension for final processed data. This structured approach ensures seamless integration with downstream tools and simplifies data sharing and collaboration.

For a full breakdown of the folder structure and BIDS conventions, see [MEGAP Folder Structure Documentation](folder-structure.md).

</details>

<details>

<summary>4. I<strong>ntermediate Saving Points</strong></summary>

MEGAP saves outputs at every processing stage, allowing users to restart or resume the pipeline from any intermediate point. This capability is particularly valuable for debugging, fine-tuning parameters, or managing interruptions in large-scale analyses. Outputs from each function are systematically stored in dedicated, function-specific folders (e.g., `/filter_chpi/`), ensuring organized and easily traceable results.

The script is built to handle interruptions seamlessly, enabling users to pick up from the last completed step without reprocessing. Additionally, users can customize workflows by commenting out unnecessary steps in the main script, providing flexibility while maintaining the pipeline’s automated structure.

</details>

<details>

<summary>5. <strong>Step-Wise Results Visualization</strong></summary>

At every stage of the MEGAP pipeline, visual summaries (e.g., power spectra, sensor topography plots) are automatically generated to provide immediate feedback on the results. These plots, such as PSDs  before and after noise removal, allow users to visually verify the effectiveness of each step, ensuring transparency and confidence in the pre-processing outcomes.&#x20;

</details>

<details>

<summary>6. <strong>Verbose Files</strong></summary>

Detailed logs are created at each step, documenting progress and diagnostic information. These logs, combined with the visual outputs, enable users to validate intermediate results, troubleshoot issues, and ensure the pipeline is performing as expected.

MEGAP verbose files for each subject, saved in the `/result/verbose/` folder with the naming convention `subject.txt`. These files capture all printed outputs from every function, providing a detailed, step-by-step record of the pipeline’s execution. Each step is clearly separated by its name (e.g., `multi_taper_removal`), and the start time of the step is logged for tracking progress. Additionally, the configuration parameters used for that step are included, making it easier to debug or reproduce results.

For example, a snippet from a verbose file might look like this:

{% code overflow="wrap" %}
```
____________________multi_taper_removal____________________
Start time: 2025-01-01 12:00:00 

Config= {'min_freq': 10, 'max_freq': 260, 'freqs': None, 'method': 'spectrum_fit', 'mt_bandwidth': 4, 'p_value': 0.05}

Folder 'MEGAP/result/multi_taper_removal' created successfully.
Detected notch frequencies (Hz):
     40.00 :    1
    100.00 :   10
    150.00 :   29
10 260
Writing /MEGAP/result/multi_taper_removal/sub-CC10.fif
Closing /MEGAP/result/multi_taper_removal/sub-CC10.fif
[done]
```
{% endcode %}

</details>

<details>

<summary><strong>7. Warning for Data Quality Monitoring</strong></summary>

MEGAP includes a warning system to simplify pre-processing reporting and ensure data quality. This system generates a warning text file in the `result/warning/` folder, which flags potential issues based on user-defined thresholds. These thresholds are specified in the `pipeline_config.cfg` file under the `warning` section, allowing users to customize sensitivity for various metrics. For example:

```json
"warning": {
    "muscle": 10,        # Percentage of data affected by muscle artifacts
    "movement": 0.003,   # Maximum head movement (in meters)
    "zapline_plus": 30,  # Maximum number of line noise components removed per coil type
    "bad_channel": 6,    # Maximum number of bad channels detected
    "ica": 5             # Maximum number of ICA components rejected
}
```

If any of these thresholds are exceeded during processing, a warning is logged in the text file. For instance:

{% code overflow="wrap" fullWidth="true" %}
```markup
__________Zapline_Plus__________
Warning: zapline_plus removed 38.0 magnetometer components, which exceeds the threshold of 30 components.
This suggests the presence of significant line noise in the data. Please check the magnetometer data.

__________Muscle Artifact__________
muscle annotations covering 60.30s, more than 10.0% of total duration (400.00s). Check the data and Z-score parameter (current value: 15).

__________Bad Channel__________
Warning: 22 bad channels detected, exceeding the threshold of 6 bad channels.
There are 22 bad channels out of 248 total channels in the dataset.
Please check the bad channels and consider reprocessing or correcting the data.

```
{% endcode %}

</details>
