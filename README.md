# Project: Heifetz's Shadow (Pilot Study)
**A Structural Econometric Approach to the Dynamic Transmission of Violin Performance Styles**

> **💡 Project Context:**
> *This repository contains the **research prototype** developed for my MSc dissertation at the Royal College of Music (RCM). It serves as a supplementary code portfolio to demonstrate the implementation of **Music Information Retrieval (MIR)** and **Econometric Modeling** pipelines used to validate the research hypothesis.*

## 1. Overview
This project aims to quantify the "Thesis-Antithesis" evolution in 20th-century violin performance (e.g., Heifetz vs. Szeryng) by bridging audio signal processing with time-series econometrics. The Python prototype hosted here was developed to test the feasibility of this interdisciplinary framework.

![Stylistic Evolution Hypothesis](figure2_decay.png)
*Figure 1: Hypothetical Visualization of Stylistic Transmission. The graph illustrates the expected output of the State-Space analysis. Note how the 1st Generation's trajectory (Blue) initially converges with the source's (Red) but eventually diverges, a pattern echoed by the 2nd Generation (Green), illustrating the decay of influence over time.*

## 2. Methodology (Pilot Implementation)
To validate the model structure, a pilot test was conducted with a purposive sample of 4 seminal recordings (N=4) of Bach's *Chaconne*.

* **Feature Extraction:** Used `librosa` to extract the 'Golden Trio' features:
    * `V_Pitch`: Vibrato intensity (F0 Standard Deviation via pYIN)
    * `V_Timbre`: Tonal brightness (Spectral Centroid)
    * `V_Rhythm`: Rhythmic flexibility (Inter-Beat Interval SD)
* **Statistical Modeling:** Implemented a **Dynamic Factor Model (DFM)** within a State-Space Framework using `statsmodels` (leveraging the Kalman Filter for likelihood estimation).
    * **Auto-Selection Logic:** The algorithm compares Model A ($k=1$) and Model B ($k=2$) via AIC to objectively determine the optimal structure.
 
![State Space Model Diagram](figure1_structure.png)
*Figure 2: The Transmission Mechanism. This diagram provides a schematic representation of the proposed State-Space Model. It translates the statistical parameters into their musicological equivalents, illustrating how the **Heifetz Style** is transmitted, decayed, and observed over time. It illustrates the core hypothesis: The unobservable **Stylistic State** (α) is passed down with a decay factor (ϕ), while each recording (**Y**) is a noisy snapshot of that style. `η_t` represents the violinist's new creative input. This schematic corresponds to the `statsmodels.tsa.DynamicFactor` implementation in the code, where `α_t` maps to the unobservable stylistic state derived from the observed audio features (`Y_t`) extracted from the recordings.*

## 3. Preliminary Results (N=4)
The prototype successfully processed the audio data, and the algorithm **autonomously selected the 2-Factor Model** as the optimal structure.
* **Model Selection:** Model B ($k=2$, AIC=28.30) significantly outperformed Model A ($k=1$, AIC=36.51), empirically supporting the proposed dialectical evolution.
* **Semantic Validation:** Factor Loadings confirmed that Factor 2 acts as a "Control Factor" (suppressing vibrato/rubato), aligning with the historical "Objectivist" style.

## 4. Tools Used
The following libraries were utilized to implement the research pipeline:
* **Language:** Python 3.10
* **Audio Processing:** Librosa (Feature extraction & Signal processing)
* **Econometrics:** Statsmodels (Kalman Filter & Dynamic Factor Models)
* **Data Management:** Pandas & NumPy


## 5. Current Limitations & Future Roadmap
This pilot serves as a **Proof of Concept** to validate the computational pipeline. The following methodological limitations are acknowledged and will be rigorously addressed in the full dissertation:

1.  **Sample Size & Stability:**
    Due to the pilot sample size ($N=4$), the Hessian matrix is currently near-singular. The full-scale research will utilize the RCM archive ($N \approx 50\text{--}100$) to ensure statistical robustness and parameter convergence.

2.  **Technological Bias in Timbre (The "Vintage" Effect):**
    The current `V_Timbre` (Spectral Centroid) metric is confounded by the bandwidth limitations of historical recording equipment.
    * **Solution:** The final methodology will incorporate **High-Cut Filters (Low-Pass)** on modern recordings to normalize the frequency response across all eras, ensuring that the model measures *stylistic brightness* rather than *fidelity improvements*.

3.  **Time-Step Irregularity:**
    The standard `DynamicFactor` model assumes fixed time intervals (discrete steps). However, historical recordings occur at irregular gaps (e.g., 15 years vs. 30 years).
    * **Solution:** The full study will refine the State-Space formulation to either account for **variable time-deltas ($\Delta t$)** within a continuous-time framework or redefine the time unit ($t$) as discrete **"Pedagogical Generations"** to ensure the Transition Matrix ($T$) is accurately estimated.

---
*Author: Minsuk Lee (MSc Candidate, Royal College of Music; BA, University of Cambridge)*
