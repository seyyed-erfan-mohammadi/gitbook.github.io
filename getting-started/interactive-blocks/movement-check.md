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

# Movement Check

Head position plotting in MEGAP is done by visualizing the trajectory of the head relative to the device’s coordinate system throughout the recording session.&#x20;

In the plotting process, we use the `plot_head_positions` function to generate a visualization of the head position over time. The plot is then enhanced by adding horizontal lines that represent both the original and average head positions. Specifically, the green horizontal line indicates the original head position (based on the initial head transformation), and the red horizontal line represents the average head position calculated across all time points.

```python
original_head_dev_t = transforms.invert_transform(data.info["dev_head_t"])
average_head_dev_t = transforms.invert_transform(compute_average_dev_head_t(data, head_pos))

# Plot the head positions
fig = viz.plot_head_positions(head_pos)

# Add horizontal lines for original and average head positions
for ax, val, val_ori in zip(fig.axes[::2], average_head_dev_t["trans"][:3, 3], original_head_dev_t["trans"][:3, 3]):
    ax.axhline(1000 * val, color="r")
    ax.axhline(1000 * val_ori, color="g")
```

The generated head position plots are saved in the `plot_head_pos` folder within the results folder.

#### Head Movement Warning

In the MEGAP pipeline, if the head movement exceeds a specified threshold, a warning is generated. This threshold is defined in the [`config_pipeline.cfg`](../../basic-information/quickstart.md#id-7.-warning-for-data-quality-monitoring) . When head movement surpasses this limit, the pipeline stores the warning in the `/warning` folder within the results directory. This allows users to easily track and address potential issues related to excessive head movement without checking verbose files.
