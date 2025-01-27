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

# Regression

In MEGAP, a regression-based method is applied after ZapLine-Plus using the [`spectrum_fit`](https://mne.tools/stable/generated/mne.filter.notch_filter.html) function from the MNE library. This approach, originally proposed by [Partha Mitra and Hemant Bokil](https://global.oup.com/academic/product/observed-brain-dynamics-9780195178081?cc=us\&lang=en&), effectively removes line noise. However, the default implementation attempts to remove line noise across the entire frequency spectrum, which can be inefficient and unnecessary for non-invasive recordings. To optimize this process, the algorithm was modified in MEGAP to operate within the frequency range of 10 to 260 Hz, as frequencies above 260 Hz are typically irrelevant, and frequencies below 10 Hz are generally less affected by line noise.

The parameters for this regression method can be customized in the `pipeline_config.cfg` file under the `"multi_taper_removal"` section. Users can adjust the settings as follows:

```json
"multi_taper_removal": {
    "min_freq": 10,
    "max_freq": 260,
    "freqs": null,
    "method": "spectrum_fit",
    "mt_bandwidth": 4,
    "p_value": 0.05
}
```
