# SESTM-Lite-Chinese: Sentiment-based Stock Return Prediction via Supervised Text Screening (MVP)

## 📌 Project Overview

This project is a lightweight **Minimum Viable Product (MVP)** validation of the core logic presented in the paper:  
***Enhancing Investment Decisions with Sentiment Analysis: A Probabilistic Ranking Framework*** (Ke, Kelly & Xiu, published in the **Journal of the American Statistical Association (JASA)**; originally titled *Predicting Returns with Text Data* as an NBER Working Paper).

Traditional financial sentiment analysis relies heavily on pre-defined, static dictionaries (e.g., Harvard-IV, Loughran-McDonald). This project implements the paper's central philosophy: **"Using realized stock returns (YYY) as a supervisory signal to dynamically screen and weigh text features (XXX)."** It provides a streamlined pipeline tailored for the Chinese A-share market.

## 🧠 Core Architecture (Algorithm Logic)

The original paper introduces a **Probabilistic Ranking Framework** (referred to as the `SESTM` algorithm in early drafts). Since the full algorithm is computationally intensive and lacks a standard open-source library, this project adopts an **Engineering Proxy Approach**:

1.  **Data Alignment:** Synchronizes A-share news headlines with next-day stock return labels (Binary: 1 for Rise, 0 for Fall/Flat).
2.  **Text Preprocessing:** Utilizes `jieba` for Chinese tokenization and standard stop-word filtering.
3.  **Supervised Screening (Core):** Instead of using a fixed dictionary, it employs a **Naive Bayes** logic to calculate the conditional probability of specific words appearing alongside positive/negative returns. This allows the model to "learn" which words (e.g., "repurchase," "upgrade," or "shortfall") actually impact A-share prices.
4.  **Signal Generation:** Aggregates word-level weights to generate a daily **Long-Short Signal** for individual stocks.

## 🚀 Quick Start

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/Wu-Stat/SESTM-Lite-Chinese.git
    ```
2.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Run the Prototype:**
    Open `notebooks/01_sestm_mvp_pipeline.ipynb` and execute the cells to run the end-to-end pipeline.

## ⚠️ Limitations & Future Work

As an MVP designed for conceptual validation, the current pipeline exhibits several known limitations which highlight the necessity of the full framework proposed by Ke, Kelly & Xiu:

### Current Limitations
* **Small Sample Overfitting & Entity Bias:** With a limited sample size, the model tends to memorize firm-specific keywords (e.g., incorrectly classifying "Moutai" or specific stock codes as general sentiment indicators) rather than extracting generalizable financial sentiment.
* **Inadequate Text Preprocessing:** The output dictionary shows that raw numbers and irrelevant entity names heavily skew the weight distribution. Standard stop-word lists are insufficient for complex financial corpora.
* **Look-ahead Bias in Evaluation:** The current reported accuracy is based on in-sample fitting. In quantitative finance, in-sample metrics are highly deceptive without strict chronological out-of-sample (OOS) backtesting.

### Future Work (Roadmap)
* **Scale the Dataset:** Expand the scraping scope to the entire CSI 300 index over a 3-5 year rolling window to ensure statistical significance.
* **Implement Marginal Screening:** Introduce the statistical screening step from the original paper to eliminate firm-specific noise and low-frequency words before training.
* **Out-of-Sample (OOS) Backtesting:** Re-architect the pipeline to split training/testing data chronologically to simulate real-world trading performance.
* **Transition to Topic Modeling:** Upgrade the core algorithm from Naive Bayes to a Supervised Topic Model (Mixture Multinomial) with Penalized MLE to fully replicate the paper's framework.

## 📚 References

* Ke, Z. T., Kelly, B., & Xiu, D. (2026). Enhancing Investment Decisions with Sentiment Analysis: A Probabilistic Ranking Framework*. Journal of the American Statistical Association, 1–26. https://doi.org/10.1080/01621459.2026.2643001
* *Earlier Draft:* Ke, Zheng and Kelly, Bryan T. and Xiu, Dacheng, Predicting Returns with Text Data (September 30, 2020). University of Chicago, Becker Friedman Institute for Economics Working Paper No. 2019-69, Yale ICF Working Paper No. 2019-10, Chicago Booth Research Paper No. 20-37, Available at SSRN: https://ssrn.com/abstract=3389884 or http://dx.doi.org/10.2139/ssrn.3389884

---
*Disclaimer: This project is intended for academic research and lightweight logic verification only. It does not constitute investment advice.*