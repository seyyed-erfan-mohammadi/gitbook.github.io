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

1. **Automated Execution**\
   MEGAP runs seamlessly from start to finish without requiring manual intervention, minimizing human error and accelerating workflows. This ensures consistency across datasets, even in studies with hundreds of participants.
2. **Standardization**\
   Parameters are pre-configured based on established guidelines and peer-reviewed studies, ensuring alignment with best practices in MEG research. This standardization guarantees reliable outcomes while maintaining methodological rigor.
3. **Configuration Files**\
   While defaults are optimized for most datasets, MEGAP allows users to adjust parameters via customizable configuration files. This flexibility ensures adaptability to unique experimental needs without compromising the pipeline’s structure.
4. **Organized Output**\
   Results are systematically organized into participant-specific folders, with final outputs adhering to the Brain Imaging Data Structure (MEG-BIDS) standard. This simplifies data sharing, collaboration, and integration with downstream analysis tools.
5. **Intermediate Saving Points**\
   MEGAP saves outputs at each processing stage, enabling users to restart or resume the pipeline from any intermediate point. This feature is invaluable for debugging, optimizing parameters, or handling interruptions in large-scale analyses.
6. **Step-Wise Results Visualization**\
   Each pre-processing step generates visual summaries (e.g., power spectra, sensor plots) to verify outcomes. These plots provide immediate feedback, ensuring transparency and enabling users to validate intermediate results.
7. **Verbose Files**\
   Detailed logs of all processing steps, including algorithm outputs and metadata, are saved as text files for each participant. This documentation supports reproducibility, troubleshooting, and auditability of the pipeline’s decisions.
