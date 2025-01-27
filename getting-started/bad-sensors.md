---
icon: '7'
---

# Bad sensors

### MEGIN Devices

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

The locations of bad sensors and their scores are plotted and saved in the **plot\_bad\_channel** folder.





### CTF/BTI Dvices

