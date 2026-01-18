# Simulating the Dynamics of Fairness: Auditing Feedback Loops in Ranking Systems

> **A pilot project illustrating how feedback loops in ranking systems can lead to unequal exposure over time, and how simple dynamic interventions can mitigate this.*

## 📌 Project Overview
This project investigates the **"Dynamics of Fairness"** in algorithmic ranking systems. Standard recommendation algorithms often rely on feedback loops (e.g., clicks, views) that create a **"Rich-get-Richer"** phenomenon. Over time, high-quality items from marginalized groups (outliers) can be effectively erased from the system due to a lack of initial exposure.

To audit and mitigate this, I built a simulation that:
1.  Uses **NLP (DistilBERT)** to derive "True Quality" scores from real-world text data (IMDb reviews).
2.  Models the recommendation process as a **Dynamical System** with feedback loops.
3.  Implements a mathematical intervention using **Dynamic Weighting** to restore equity over time.

## 🚀 Motivation
As stated in my research statement, fairness is not a static metric but a **moving target**. Adjusting a system for one group often creates dynamic trade-offs. This project aims to visually demonstrate how:
*   Standard algorithms **erase outliers** (high-quality but low-exposure items).
*   **Operationalizing normative values** (e.g., Equity of Attention) into mathematical constraints can fix these loops without sacrificing system utility.

## 🛠 Methodology

### 1. Data & NLP Scoring
Instead of using synthetic random numbers, I used the **IMDb dataset** to simulate real-world content.
*   **Model:** `distilbert-base-uncased-finetuned-sst-2-english`
*   **Process:** The model analyzes the sentiment of each review to assign a continuous `True Quality` score (0.0 to 1.0). This represents the intrinsic merit of the content.

### 2. Simulation Logic
The simulator runs two parallel worlds for `300` time steps:

*   **🔴 Naive Model (The Problem):**
    *   Follows a standard popularity-based ranking: $Score_t = Quality + \alpha \cdot Exposure_t$
    *   This simulates the **Winner-Takes-All** dynamic where early exposure dictates long-term success.

*   **🟢 Fair Model (The Intervention):**
    *   **Operationalizes Amortized Fairness**: Ensures that cumulative exposure is proportional to item quality over time.
    *   **Mechanism:** It calculates the *attention deficit* for each item and applies a dynamic boost:
        $$Score_t = Quality + \lambda \cdot (TargetExposure_t - ActualExposure_t)$$
    *   This acts as an control mechanism to rescue under-exposed high-quality items.

## 📊 Key Findings

![Simulation Results](/Fairness-Dynamics-Simulator/figure/results.png)
*(Fig 1. Comparison of Naive vs. Fair Simulation Results)*

The results visually confirm the hypothesis regarding the **"Erasure of Outliers"** and the efficacy of the intervention.

### 1. The Erasure of Outliers (Right Plot - Red Dots)
*   In the **Naive** setting (Red), the system exhibits severe inequality.
*   Notice the red dots on the bottom right: numerous items with **high True Quality (> 0.3)** received **ZERO exposure**.
*   This proves that without intervention, the feedback loop "erases" valid outliers simply due to bad luck in initial conditions.

### 2. Operationalizing Equity (Right Plot - Green Dots)
*   The **Fair** intervention (Green) successfully restored equity.
*   The green dots form a linear correlation, meaning **Exposure is proportional to True Quality**.
*   This demonstrates that mathematical interventions can enforce **"Equity of Attention"**, ensuring that merit—not just popularity—drives long-term visibility.

### 3. Mitigating Inequality (Left Plot - Lorenz Curve)
*   The Naive model (Red line) shows extreme inequality (Gini coefficient approaches 1).
*   The Fair model (Green line) significantly reduces inequality while maintaining a meritocratic distribution (it does not enforce perfect equality, which would be undesirable in ranking).


   