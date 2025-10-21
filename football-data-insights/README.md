# Beating the Odds: Football Data Insights

*"Some people believe football is a matter of life and death — I’m very disappointed with that attitude. I can assure you it is much, much more important than that."*  
— **Bill Shankly (1913–1981)**, legendary Liverpool manager  

---

## ⚽ Introduction
This project began with a simple — and probably universal — ambition among football fans: the idea that with enough logic, spreadsheets, and late-night inspiration, one might finally find the pattern that beats the bookmakers.  
What started as a hobby soon turned into a long experiment in probability. I tested everything from Poisson models and Bayesian refinements to Bale’s expected-goal ratios, Elo-style form factors, and a small forest of Excel formulas that should probably remain classified.  

Somewhere along the line, it stopped being about betting and became about understanding.  
I didn’t find the secret to endless profit or a one-way ticket to Hawaii — but I did find something better: a clear, data-driven picture of how football markets really work, how patterns hide inside the noise, and how analytics can turn a fan’s illusion into a structured, measurable reality.

---

## 🎯 Objective
To explore whether a football betting model can achieve consistent profitability once bookmaker margins are included — and to demonstrate how **data analysis and visualization** can expose structure and logic in a system that appears purely unpredictable.

---

### 📂 Project Files
- [📄 Project Summary (PDF)](./Football_Data_Insights_Summary.pdf)
- [📊 Power BI Dashboard (.pbix)](./Football_Predictions_Public.pbix)

---
## 🧮 Dataset & Methodology
- **Scope:** 494,274 matches (2019–2024) across nearly 300 competitions.  
- **Source:** Primarily from [OddsPortal.com](https://www.oddsportal.com/), cross-checked with open football databases.  
- **Preparation:** The most time-consuming phase — resolving club renamings, league rebrands, missing fixtures, duplicate records, inconsistent date formats, and multilingual naming.  
- **Normalization:** Bookmaker (market) odds were converted into **Fair Odds** by removing margin overrounds, ensuring total probabilities summed to 100 %.  

This cleaning process produced a dataset that could be analyzed with confidence — large-scale, coherent, and transparent.

---

## 📈 Modeling Approach
The model relies primarily on the **Poisson distribution**, widely used in sports analytics to estimate goal counts.  
It assumes goals occur as independent events following a predictable statistical pattern. From the expected goal averages of both teams, it calculates the likelihood of every possible scoreline and derives the probability of **Home Win**, **Draw**, or **Away Win**.

Additional experiments included:  
- **Bayesian adjustments** for dynamic weighting  
- **Bale’s xG ratio** and **Elo-style form metrics**

Ultimately, the Poisson framework proved the most stable and transparent for large-scale comparison with bookmaker data.

---

## 🖥️ Dashboard Overview
### **Beating the Odds — Interactive Power BI Dashboard**
The Power BI dashboard connects model probabilities and bookmaker expectations across nearly half a million matches.  
It visualizes how mathematics, form, and market margins interact to shape profitability.

**Main components:**
- ROI (%) and Hit Rate (%) for Home / Draw / Away outcomes  
- Market vs Fair ROI results in €  
- Interactive map and filters for form difference, odds range, competition, and season  
- Contextual tooltips explaining each metric and toggle  

The result is a **live analytical simulator**: a transparent way to explore how mathematics, market behavior, and margins collide in real time.

---

## 🧪 How It Works
1. The model simulates a €1 stake on every outcome (Home, Draw, Away) for every match — a “full-market” baseline.  
2. **ROI, Hit Rate, and Profit/Loss** metrics update instantly when filters change.  
3. Users can test hypotheses such as betting only when both Poisson and market odds favor the same side.  
4. Each metric updates dynamically, showing the impact of strategy filters on bankroll performance.  

> The dashboard becomes not a betting tool, but a **probability laboratory** — revealing how rational systems behave under irrational hopes.

---

## 🔍 Key Insights
- **Predictability exists — profitability does not.**  
  Statistical models outperform random guessing, but bookmaker margins erase the gain.  
- **Margins neutralize advantage.**  
  Certain odds clusters briefly show ROI > 100 %, but the edge fades as markets self-correct.  
- **The paradox.**  
  Football is chaotic in detail yet orderly in mass — predictable collectively, elusive individually.  

You can understand the market, but not outsmart it for long — and that, paradoxically, is the real satisfaction.

---

## 🧠 Collaboration
Initial data collection and early testing were developed jointly with a long-time friend.  
All dashboard design, modeling, analysis, and documentation are entirely my own work.

---

## ⚠️ Disclaimer
This is a **data-analytics and visualization project**, not a betting system.  
All ROI and profit figures are **simulated and educational**, illustrating statistical reasoning and market efficiency — not financial performance.

---

## 🏁 Conclusion
In a theoretical fair-odds world, the model would yield consistent profit.  
In the real one, bookmaker margins quietly absorb every mathematical advantage.  

That’s the true reward of the project: transforming a futile chase for profit into a structured exploration of **probability, logic, and market behavior**.  
You can’t beat the odds — but you can learn everything about how they work.  
And sometimes, that’s the more satisfying win.

---

## 🧾 External Evaluations
<details>
<summary>Gemini Evaluation (2025)</summary>
“The project transforms a futile chase for profit into a structured exploration of probability, logic, and market behavior. It excels in scope, methodology, and clarity — an exemplary demonstration of using analytics to understand the limits of a competitive market.”
</details>

<details>
<summary>Claude Review (2025)</summary>
“You created something rare: a project that failed at its original goal but succeeded as an analytical demonstration. The humility in the conclusion marks genuine scientific thinking. A strong portfolio piece for sports analytics or data science roles.”
</details>

<details>
<summary>Grok Review (2025)</summary>
“‘Beating the Odds’ bridges data analytics and football betting with honesty and rigor. Its strength lies in comprehensive data, robust methodology, and engaging visualization. Educational, balanced, and intellectually honest.”
</details>

<details>
<summary>DeepSeek Review (2025)</summary>
“An outstanding example of data analytics. It demonstrates the full data-science lifecycle — from collection and cleaning to modeling and visualization. The project’s honesty is its success.”
</details>

<details>
<summary>Perplexity Summary (2025)</summary>
“Shows that while football betting patterns can be modeled, bookmakers’ margins neutralize statistical advantages. The value lies in understanding, not gaming, the market.”
</details>

<details>
<summary>ChatGPT Review (2025)</summary>
“A rare blend of technical rigor and intellectual honesty — turning modeling and visualization into a story about understanding rather than prediction. Clarity and humility define true data literacy.”
</details>

---

### 🧠 License & Goodwill
This work is free to use and share.  
If so, mention me in goodwill — like buying me a virtual beer 🍺  

© 2025 Károly Boczka
