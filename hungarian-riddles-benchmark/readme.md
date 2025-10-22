🇭🇺 Hungarian Riddles Benchmark

A No-Code Evaluation Project
Developed by Károly Boczka
Multilingual AI Evaluator & Data Analyst — October 2025

📘 Overview

This project introduces a Hungarian benchmark evaluation designed to demonstrate and practice no-code model evaluation skills.
It contains 100 metaphorical, trivia-style riddles in Hungarian, covering culture, history, sports, geography, daily life, politics, literature, and the arts.

The riddles were intentionally created to test factual accuracy, reasoning clarity, and language quality across multiple LLMs.

👥 Human Baselines
Baseline	Description	Accuracy
A	University-educated male, 50+ years	95/100
B	18-year-old male	50/100

These human reference points serve as comparison anchors for model performance.

⚙️ Evaluation Setup

Each of the 100 riddles was presented to six large language models (LLMs) with the following strict instructions:

Answer in Hungarian

Provide a concise justification (≤ 2 sentences)

Avoid speculation or multiple alternatives

Use web search when possible

Provide one final answer, no clarifying questions

🧮 Evaluation Rubric (0–5 scale)
Category	Criteria	Scoring
1️⃣ Factual Accuracy	Correctness of the answer	0 / 1 / 3 / 5
2⃣ Justification	Logical and factual reasoning	0–5
3️⃣ Grammar & Style	Correctness, conciseness, and cultural clarity	0–5
Auto-0 Rule	If the answer was not in Hungarian or had no justification → total score = 0	

All scores were processed and visualized in Power BI dashboards for trend analysis and model comparison.

🧠 Model Selection (September 2025)
Label	Model
A	GPT-5 Deep Search
B	Claude Sonnet 4
C	Gemini 2.5 Flash
D	Grok
E	GPT-5
F	DeepSeek R1

This mix contrasts standard reasoning with search-augmented approaches and diverse architectures.

📊 Key Findings

No model outperformed the top human baseline (95%).

Model A (77%) was the only one to exceed the lower baseline (50%).

Models C (53%) and E (49%) performed mid-range.

Models B (38%), D (32%), and F (33%) trailed below the human baseline.

Cultural and linguistic nuance heavily influenced performance — even the strongest model hallucinated on 8% of riddles, while the weakest exceeded 50%.

Summary Table
Model	Overall	Factual	Justification	Language	Hallucination
A	67.7%	77%	60%	56%	8%
B	41.5%	38%	40%	66%	41%
C	55.9%	53%	45%	67%	14%
D	35.5%	35%	35%	45%	36%
E	45.0%	49%	30%	44%	16%
F	33.5%	32%	24%	52%	58%
⚠️ Limitations

This was a single-rater, single-session pilot benchmark — not a scientific study.
Future versions will explore:

Multi-prompt and multi-session setups

Larger sample sizes

Multilingual extensions (HU-EN-HR)

Session drift was observed — suggesting minor underestimation of late-session performance.

📂 Data Access

All raw responses (600 items), answer keys, and evaluation tables are included in this GitHub repository.

🧩 Framing Disclaimer

This benchmark is an independent personal project, meant to showcase evaluator methodology:

Structured task design

Consistent rubric application

Transparent result communication

Model names are revealed only once; analysis focuses on evaluation quality, not product ranking.

🧭 Conclusion

Factual reasoning and cultural nuance remain challenging for LLMs in smaller languages.
Human-in-the-loop evaluation continues to be essential for maintaining factual grounding and interpretive depth in Hungarian-language reasoning tasks.

©️ Credits & License

© 2025 Károly Boczka
Freely reusable with credit.
If you found this useful, mention me — or buy me a virtual beer 🍺
