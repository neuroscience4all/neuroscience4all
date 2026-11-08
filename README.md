# rs-fMRI-Triple-Network-Pipeline
A multi-modal computational neuroscience pipeline utilizing spatial Independent Component Analysis (ICA) and psychophysiological covariates to map neuro-metabolic connectivity deficits in clinical burnout and Major Depressive Disorder (MDD).

## Technical Architecture & Core Toolboxes
*   **Neuroimaging Engine:** Statistical Parametric Mapping (SPM12; MATLAB environment)
*   **Connectivity Modeling:** CONN Functional Connectivity Toolbox
*   **Spatial Normalization:** Montreal Neurological Institute (MNI) Standard Template Space
*   **Signal Processing Modality:** Resting-state fMRI (rs-fMRI) Blood-Oxygen-Level-Dependent (BOLD) time-series tracking
*   **Multi-Modal Covariates:** Physiological integration of HPA-axis (Salivary Cortisol) and Autonomic Nervous System (ECG-derived HRV) timelines

---

## 1. Methodological Pipeline & Data Flow

Use code with caution.
[ Raw rs-fMRI & ECG/Saliva Data ]
│
▼
[ SPM12 Preprocessing Pipeline ]
Realignment ➔ Slice-Timing ➔ MNI Normalization ➔ Smoothing
│
▼
[ Denoising & Physiological Artifact Gating ]
Anatomical CompCor + HRV/ECG Vector Regressors
│
▼
[ Spatial Independent Component Analysis ]
Group-ICA Decomposition (FastICA Algorithm)
│
▼
[ Dual Regression & Second-Level GLM ]
Covariates: Age, Sex, Cortisol Curve (CAR), Symptom Chronicity

### Preprocessing Specifications (SPM12 / CONN)
1. **Functional Realignment:** Head motion tracking and correction using mean Framewise Displacement (FD) thresholds (Power et al., 2012) to isolate micro-movement confounding variables.
2. **Slice-Timing Correction:** Temporal adjustment for temporal shifts in interleaved slice acquisitions.
3. **Co-registration & Segmentation:** Structural T1-weighted anatomical volumes matched to functional BOLD time-series, with high-resolution segmentation of Gray Matter, White Matter, and Cerebrospinal Fluid (CSF).
4. **Spatial Normalization:** Affine and non-linear transformations warping individual data into standard MNI coordinate space.
5. **Spatial Smoothing & Temporal Filtering:** Application of a Gaussian kernel and band-pass filtering to optimize signal-to-noise ratio prior to network configuration.

### Denoising & Extraneous Variable Control
*   **Anatomical CompCor:** Regression of non-neural noise artifacts by extracting principal components from segmented White Matter (WM) and CSF masks (Behzadi et al., 2007).
*   **Physiological Covariates:** Structural insertion of time-domain Heart Rate Variability (HRV; e.g., RMSSD) from integrated ECG timelines, paired with Cortisol Awakening Response (CAR) tracking curves. This isolates central neural connectivity profiles from peripheral autonomic and endocrine exhaustion.

---

## 2. Theoretical Framework: The Triple-Network Deficit Model
This analytical pipeline shifts beyond traditional single region-of-interest configurations to map macro-scale system interactions governed by localized micro-scale molecular pathology (cortisol-induced cAMP/PKA/CREB pathway dysregulation and downstream BDNF down-regulation).

*   **Salience Network (SN):** Anchored by the Bilateral Anterior Insula (AI) and the midline dorsal Anterior Cingulate Cortex (dACC). Acts as the homeostatic control switchboard.
*   **Default Mode Network (DMN):** Medial Prefrontal Cortex (mPFC) and Posterior Cingulate Cortex (PCC). Governs internal rumination and self-referential narratives.
*   **Central Executive Network (CEN):** Anchored by the Dorsolateral Prefrontal Cortex (dlPFC). Manages executive functioning, task-control, and working memory.

The statistical model isolates how the Right Anterior Insular node fails to properly down-regulate the DMN and up-regulate the dlPFC under states of chronic occupational stress, providing an empirical biological boundary separating Burnout from clinical MDD.

---

## 3. Second-Level Statistical Analysis Plan (SAP)
Group-level differences across stratified clinical cohorts (Physician Burnout vs. Major Depressive Disorder vs. Healthy Controls) are executed within a General Linear Model (GLM) framework.

[ Second-Level GLM Matrix Configuration ]
Y_connectivity = β0 + β1(Group) + β2(Age) + β3(Sex) + β4(Cortisol_AUC) + β5(Symptom_Duration) + ε

*   **Primary Omnibus Test:** Second-level F-tests identifying overall spatial variance across the three independent sample networks.
*   **Planned Contrasts:** Directional t-contrasts (e.g., evaluating localized Right AI-to-dlPFC decoupling thresholds).
*   **Multiplicity Correction:** Stringent False Discovery Rate control implemented via Benjamini-Hochberg procedures ($p_{FDR} < .05$).
