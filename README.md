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
| **4** | LLM benchmark — 10 models | ⏳ In progress |
| **5** | Dashboard | 🔜 Planned |

---

## 🔬 Methodology

### Phase 1 — Data
* **20,800+ reviews** scraped from Google Maps and TripAdvisor across 72 restaurants in Nimman.
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

### Phase 4 — LLM Benchmark (in progress)
10 models evaluated on the Gold Standard using identical prompts, temperature=0, via OpenRouter.

| # | Model | Provider | Tier |
| :--- | :--- | :--- | :--- |
| 1 | **Gemini 2.5 Flash** | Google | ⚡ Fast |
| 2 | **Gemini 2.5 Pro** | Google | 💎 Flagship |
| 3 | **GPT-4o mini** | OpenAI | ⚡ Fast |
| 4 | **GPT-4o** | OpenAI | 💎 Flagship |
| 5 | **Claude Haiku 4.5** | Anthropic | ⚡ Fast |
| 6 | **Claude Sonnet 4.6** | Anthropic | 💎 Flagship |
| 7 | **Llama 3.3 70B** | Meta | OSS |
| 8 | **Qwen 2.5 72B** | Alibaba | OSS · Thai-best |
| 9 | **DeepSeek V3.1** | DeepSeek | OSS · MoE |
| 10 | **DeepSeek R1** | DeepSeek | OSS · Reasoning |

* **Primary metric:** Macro F1 per aspect. Also reporting Cohen's κ and confusion matrices.
* **Next Step:** Best model → applied to full 20,800+ review corpus to compute NASS (Nimman Aspect Sentiment Score) per restaurant.

---

## 💡 Key findings so far

* **Service** has the highest marginal effect on perceived value (β = 0.487) — more than Food or Atmosphere.
* **Atmosphere** had notably more annotator disagreement than other aspects (77% unanimous vs 94.9% for Price), reflecting how subjective ambience language tends to be.
* No model in this benchmark has published Thai-specific ABSA results — this experiment generates new data.

*(LLM benchmark results to be added after Phase 4)*

---

## 🛠️ Stack

| Task | Technology |
| :--- | :--- |
| **Scraping** | Python · `BeautifulSoup` / `Selenium` |
| **Processing** | Python · `Pandas` |
| **Modeling** | Python · `statsmodels` |
| **Annotation QA** | Python · `sklearn` (Fleiss' κ, Cohen's κ) |
| **LLM Benchmark** | OpenRouter API |
| **Dashboard** | Streamlit *(planned)* |

---

## 📚 References

* Lancaster, K. J. (1966). A new approach to consumer theory. *Journal of Political Economy*, 74(2), 132–157.
* Pontiki, M. et al. (2014). SemEval-2014 Task 4: Aspect Based Sentiment Analysis. *SemEval 2014*.
* Landis, J. R., & Koch, G. G. (1977). The measurement of observer agreement for categorical data. *Biometrics*, 33(1).

---
**Tak Thongsoet** · [LinkedIn](https://www.linkedin.com/in/tak-thongsoet) · [GitHub](https://github.com/sosayS0)
