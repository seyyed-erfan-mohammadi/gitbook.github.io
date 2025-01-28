---
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

# Zapline\_Plus

The core algorithm of ZapLine-Plus is implemented in MATLAB. To ensure seamless automation within MEGAP, the pipeline interfaces with MATLAB to process outputs from earlier stages and then returns the results to Python for further analysis. This integration is achieved through the MATLAB-Engine library.

MEGAP uses the following code to execute MATLAB and retrieve the number of components removed during line noise suppression, which is then used for warning purposes:

```python
eng = matlab.engine.start_matlab()
zapline_output = eng.zapline_matlab(
    main_location, subject_id, grad_channel_indices, mag_channel_indices, 
    chpi_frequencies, powerline_noise_freq, sampling_rate
)
output_array = np.array(zapline_output)

# Access Nrem_mag and Nrem_grad
Nrem_mag = output_array[0][0]
Nrem_grad = output_array[0][1]

```

MEGAP incorporates a MATLAB script that uses the MNE-MATLAB library for reading data and performing ZapLine-Plus. If cHPI frequencies are present, ZapLine-Plus is configured to process and remove line noise harmonics up to the last powerline noise harmonic before the lowest cHPI frequency. By default, the maximum harmonic is set to 260 Hz if no cHPI frequencies are provided. The script starts detecting and removing line noise from 10 Hz.

Below is the MATLAB code snippet for determining the maximum harmonic:

```matlab
try
    % Calculate harmonics
    harmonics = powerline_noise * (1:floor(min(chpi_freqs) / powerline_noise));
    
    % Find the maximum harmonic
    max_harmonic = max(harmonics) + 1;
    
    % Display the results (optional)
    disp('Harmonics calculated successfully.');
    disp(['Harmonics: ', num2str(harmonics)]);
    disp(['Max harmonic: ', num2str(max_harmonic)]);
catch ME
    max_harmonic = 260; % Assign a default value
    disp('Setting max_harmonic to default value: 260');
end

```

In MEGAP, a customized version of the ZapLine-Plus main script is included, where the operations for determining the minimum and maximum frequencies have been modified. When MEGAP runs, this script is automatically placed in the `zapline_plus` folder to ensure it integrates seamlessly into the pipeline.

{% hint style="info" %}
If you need to adjust the minimum frequency operation, you can modify the script `clean_data_with_zapline_plus.m`, located in the MEGAP folder.
{% endhint %}

#### Zapline\_plus Warning

In MEGAP, a warning will be triggered during the zapline\_plus line noise removal process if the number of removed components for each coil type exceeds the specified threshold. The warning will be stored in the "warning" section of the results directory, allowing users to monitor and address potential issues with the line noise removal procedure. The threshold can be adjusted in the configuration file.

