---
icon: '5'
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

# Line noise

Dealing with line noise in MEG data requires effective suppression methods to avoid compromising data quality. While traditional low-pass or notch filters are commonly used, they can eliminate valuable information in the line noise frequency range, limiting their applicability. Advanced techniques like frequency-domain regression (e.g., CleanLine) offer alternatives but are less effective in handling amplitude and phase fluctuations. Recent methods like Zapline and its enhanced version, [ZapLine-Plus](https://onlinelibrary.wiley.com/doi/full/10.1002/hbm.25832), provide more refined approaches, addressing non-stationary noise and automating parameter selection. MEGAP integrates ZapLine-Plus using MATLAB-Engine for processing. To further suppress residual noise, a regression-based method is applied post-ZapLine-Plus, optimized for speed and relevance by focusing on the same frequency range.

<figure><img src="../../.gitbook/assets/figure 4.png" alt=""><figcaption><p>Comparison between the effect of different stages of line noise removal (A) before removing any line noise, (B) after removing line noise with ZapLine_plus and (C) using regression-based method in addition to ZapLine_plus</p></figcaption></figure>
