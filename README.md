# Nimman Price Perception Insight (NPPI)

> Quantifying the implicit value of restaurant attributes in Nimman, Chiang Mai — using customer reviews, OLS regression, and a multi-LLM benchmark for Thai-English sentiment analysis.

**Senior Research Project** · Faculty of Economics, Chiang Mai University · 2026

---

## 📌 What this project does

Customers don't just pay for food. They pay for atmosphere, service, and the feeling that it was worth it. This project tries to put numbers on that. 

By treating restaurant reviews as economic data, **NPPI** decomposes overall perceived value into attribute-level components — then asks which improvements actually move the needle on customer perception. 

Two lenses are used:
* **Hedonic model** — structured data (star ratings) → *"which attribute drives overall rating the most?"*
* **ABSA via LLM** — unstructured text (review content) → *"what do customers actually say they feel?"*

---

## 🚀 Status

| Phase | Description | Status |
| :--- | :--- | :---: |
| **1** | Data collection & engineering | ✅ Done |
| **2** | Hedonic regression modeling | ✅ Done |
| **3** | Gold standard annotation (100 reviews, 3 annotators) | ✅ Done |
| **4** | LLM benchmark — 10 models | ✅ Done |
| **5** | Dashboard | ✅ Done |

---

## 🔬 Methodology

### Phase 1 — Data
* **15,800+ reviews** scraped from Google Maps and TripAdvisor across 72 restaurants in Nimman.
* Capped at 210 reviews per restaurant for corpus balance.
* **Metadata:** rating, date, platform, restaurant.

### Phase 2 — Hedonic Model
Applied Lancaster's Attribute Theory (1966) + OLS regression to quantify how much each attribute contributes to overall perceived value.

> **Avg_Rating** = 0.148 + 0.298(Food) + 0.487(Service) + 0.184(Atmosphere) − 0.027(Price_Level)
> 
> *R² = 0.81 · N = 72 restaurants*

The β coefficients are read as relative importance weights — **Service** has the strongest marginal effect, followed by **Food**, then **Atmosphere**. Price Level was not significant (p = 0.23), likely due to the narrow range in this dataset.

### Phase 3 — Gold Standard
Built a 100-review labeled dataset for evaluating LLM performance:
* 3 independent annotators labeled each review across 4 aspects (Food / Service / Atmosphere / Price) using `POS` · `NEU` · `NEG` · `N/A`.
* **Inter-annotator agreement:** Fleiss' κ = 0.73+ (Substantial, per Landis & Koch 1977).
* Conflicts resolved by majority vote (54 cases) or lead researcher adjudication (2 cases), following SemEval ABSA conventions (Pontiki et al., 2014).

**Label distribution:**

| Aspect | POS | NEG | NEU | N/A |
| :--- | :---: | :---: | :---: | :---: |
| **Food** | 72 | 6 | 7 | 15 |
| **Service** | 35 | 12 | 2 | 51 |
| **Atmosphere** | 46 | 6 | 3 | 45 |
| **Price** | 14 | 4 | 3 | 79 |

### Phase 4 — LLM Benchmark

10 models evaluated on the Gold Standard using identical prompts, temperature=0, via OpenRouter.
Metrics: Macro F1, Weighted F1, Accuracy, and Cohen's κ per model × aspect.

**Results (ranked by Avg Macro F1):**

| Rank | Model | Provider | Avg Macro F1 | Avg κ |
|---|---|---|---|---|
| 1 | Gemini 2.5 Pro | Google | 0.817 | 0.821 |
| 2 | Gemini 2.5 Flash | Google | 0.799 | 0.810 |
| 3 | Claude Sonnet 4.6 | Anthropic | 0.783 | 0.781 |
| 4 | Llama 3.3 70B | Meta | 0.775 | 0.787 |
| 5 | Claude Haiku 4.5 | Anthropic | 0.770 | 0.772 |
| 6 | DeepSeek V3.1 | DeepSeek | 0.760 | 0.779 |
| 7 | GPT-4o | OpenAI | 0.758 | 0.791 |
| 8 | DeepSeek R1 | DeepSeek | 0.749 | 0.791 |
| 9 | Qwen 2.5 72B | Alibaba | 0.705 | 0.739 |
| 10 | GPT-4o mini | OpenAI | 0.704 | 0.743 |

Full per-class precision/recall/F1 and confusion matrices available in the evaluation report.
---

## 💡 Key findings so far

* **Service** has the highest marginal effect on perceived value (β = 0.487) — more than Food or Atmosphere.
* **Atmosphere** had notably more annotator disagreement than other aspects (77% unanimous vs 94.9% for Price), reflecting how subjective ambience language tends to be.
* No model in this benchmark has published Thai-specific ABSA results — this experiment generates new data.

### Next Step
Applied Gemini 2.5 Flash to the full 20,800+ review corpus to compute NASS 
(Nimman Aspect Sentiment Score) per restaurant — chosen over the top-ranked Gemini 2.5 Pro 
under a predefined decision rule (select the cheaper model if the Macro F1 gap to the top 
performer is under 0.03). Flash's gap of 0.018 cleared that threshold, delivering an 
estimated ~90% cost reduction versus Pro for a marginal accuracy trade-off.

_The hedonic OLS model (β coefficients above) is validated by an accompanying econometrics 
research paper using Google Maps' native aspect-based star ratings (N=72), independent of 
the ABSA/NASS layer described here._

---

## 🛠️ Stack

| Task | Technology |
| :--- | :--- |
| **Scraping** | Python · `BeautifulSoup` / `Selenium` |
| **Processing** | Python · `Pandas` |
| **Modeling** | Python · `statsmodels` |
| **Annotation QA** | Python · `sklearn` (Fleiss' κ, Cohen's κ) |
| **LLM Benchmark** | OpenRouter API |
| **Dashboard** | HTML/CSS/JavaScript + Chart.js (custom-built) |

---

## 📚 References

* Lancaster, K. J. (1966). A new approach to consumer theory. *Journal of Political Economy*, 74(2), 132–157.
* Pontiki, M. et al. (2014). SemEval-2014 Task 4: Aspect Based Sentiment Analysis. *SemEval 2014*.
* Landis, J. R., & Koch, G. G. (1977). The measurement of observer agreement for categorical data. *Biometrics*, 33(1).

---
**Tak Thongsoet** · [LinkedIn](https://www.linkedin.com/in/tak-thongsoet) · [GitHub](https://github.com/sosayS0)
