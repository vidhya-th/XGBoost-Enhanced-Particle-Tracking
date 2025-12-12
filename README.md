# Comparative Analysis of Particle Reconstruction: Kalman Filter vs. XGBoost

##  Project Overview

This study conducts a comparative analysis of two distinct methodologies—the classical **Kalman Filter (KF)** and a **Machine Learning (ML)** approach using Extreme Gradient Boosting (XGBoost)—for the task of **muon momentum reconstruction** in the Iron Calorimeter (ICAL) experiment.

The project evaluates both algorithms based on momentum resolution and computational efficiency using ideal, simulated neutrino interaction data.

##  Methodology

Two primary approaches were developed and analyzed for reconstructing the input momentum ($P_{in}$) to obtain the reconstructed momentum ($P_{rec}$).

### 1. Classical Kalman Filter (KF)

The Kalman filter is employed for estimating the momentum and charge of muons.

* **Process:** The algorithm recursively propagates a state vector and its error covariance matrix from one detector plane to the next, performing prediction, filtering, and smoothing steps. 
* **State Vector:** Defines the complete state of a particle in a given plane, including position, track slope, momentum magnitude, and charge.
* **Iterations:** The filtering and smoothing process is typically iterated **4-5 times** to achieve the best optimal state estimate.

### 2. Machine Learning (XGBoost) Model

A supervised machine learning model was developed using the XGBoost (Extreme Gradient Boosting) library to directly map detector hits to particle momentum.

* **Model Type:** Boosted Decision Trees (XGBoost).
* **Features:** The model uses 294 features derived from the **x and y coordinates** of the hit in each detector layer, as given by Geant-based simulation.
* **Data Representation:** Hit positions were digitized and converted into a **sparse matrix** representation (strips fired are '1', others '0'). 
* **Training Parameters:**
    * Learning Rate: 0.01
    * Maximum Depth: 10
    * Number of Estimators: 100

##  Datasets

The analysis was performed on events generated from an ideal detector model, without accounting for vertex smearing or angular smearing. The muon energy was varied linearly from **1 GeV to 10 GeV** (in steps of 1 GeV).

| Dataset | Particle | Number of events | Number of subsets | Direction Vectors |
| :--- | :--- | :--- | :--- | :--- |
| **Train-A** | $\mu^-$ | $1 \times 10^4$ | 10 | (0, -1, 0) |
| **Train-B** | $\mu^-$ | $1 \times 10^4$ | 10 | (0, 1, 0) |
| **Train-C** | $\mu^+$ | $1 \times 10^4$ | 10 | (0, -1, 0) |
| **Train-D** | $\mu^+$ | $1 \times 10^4$ | 10 | (0, 1, 0) |
| **Test-1/2/3/4** | $\mu^{\pm}$ | $1 \times 10^4$ each | 30 each | Upward/Downward |

## Results and Comparison

The performance of the algorithms was quantified by the **Momentum Resolution ($R$)**, defined as $R = \sigma / P_{in}$, where $\sigma$ is the RMS width of the $P_{rec} - P_{in}$ distribution.

| Metric | Kalman Filter (KF) | Machine Learning (XGBoost) | Comparison |
| :--- | :--- | :--- | :--- |
| **Momentum Resolution ($R$)** | *10% - 40%** | **1% - 3%** | **ML is $\times 10$ better** |
| **Mean Momentum Shift** |  -400 MeV to 600 MeV | 50 MeV to 200 MeV | ML is significantly lower/more stable |
| **Training Time** | N/A | 30 sec / $1 \times 10^4$ events | - |
| **Processing Time** | 120 min / $1 \times 10^4$ events | 1 sec / $3 \times 10^4$ events | **ML is orders of magnitude faster** |

**Conclusion:**
The XGBoost model provides significantly **better momentum resolution** than Kalman filter, a factor of 10 better than the . The XGBoost model is **substantially faster** in both training and testing/reconstruction time.

##  Future Works

The following steps are planned to build upon this baseline study and move toward a more realistic simulation:

* **Realistic Datasets:** Generate datasets with **vertex smearing** and **angular smearing**.
* **Simulation Tools:** Generate events using **Genie** (instead of simplified simulation).
* **Detector Environment:** Introduce a **non-uniform magnetic field** and incorporate detailed simulations of detector components (copper coils, RPC trays, support structures).
* **Full Event Reconstruction:** Extend reconstruction to include **hadron hits**, not just muons.
* **Hybrid Algorithm Development:** Augment the Kalman Filter by using machine learning models to perform parts of the KF process, such as prediction and filtering.
