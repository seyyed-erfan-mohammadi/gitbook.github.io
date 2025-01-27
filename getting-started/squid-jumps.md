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
    visible: true
  pagination:
    visible: true
---

# Squid Jumps

SQUID spikes, a typical form of sensor noise, manifest as sharp, sudden steps with baseline shifts. These spikes are caused by electronic instabilities affecting the SQUID state during recording, which complicates both amplitude and frequency analysis. Identifying and removing these artifacts automatically is challenging, particularly after applying spatial filters. For example, when tSSS is used immediately after recording on MEGIN systems, noise from sensors with SQUID spikes can spread to other sensors.

<figure><img src="../.gitbook/assets/figure 6.png" alt=""><figcaption><p>Example of SQUID jumps in MEG1522 sensor</p></figcaption></figure>

To mitigate this issue in MEGIN devices, the Oversampled Temporal Projection (OTP) method is applied within MEGAP. The idea of combining OTP with tSSS to improve the reduction of device-related noise was explored in [2020](https://www.sciencedirect.com/science/article/pii/S0165027020301230). By customizing the parameters of these methods, rather than relying on the default settings of tSSS, better performance in noise reduction can be achieved.

{% hint style="info" %}
OTP is used exclusively for MEGIN devices in combination with tSSS within MEGAP. It is not applied to CTF or BTI devices, as it has not been evaluated practically for these systems.
{% endhint %}
