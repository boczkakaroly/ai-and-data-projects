# Hungarian Cultural Benchmark v2.0
## Evaluating Large Language Models Beyond English

**Author:** Karoly Boczka  
**Timeline:** September - December 2025

---

## Executive Summary

This project evaluates three leading AI platforms (ChatGPT, Claude, Gemini) on 100 Hungarian cultural riddles, testing both API and UI implementations.

**The central findings:**
- UI implementations outperform APIs by 30%
- The 50-50 Reality: API implementations achieve 48% average performance while simultaneously showing 48% hallucination rates.
- Even best-performing models show 16-78% failure rates on cultural tasks
- Across 600 outputs, models generated 193 hallucinations but only 12 "I don't know" responses - a 16:1 ratio
- Models are vastly more willing to fabricate answers than admit uncertainty

These validate that **human-in-the-loop evaluation is structurally necessary** for cultural content assessment - not a temporary workaround, but a fundamental requirement.

---

## 1. Introduction: From Pilot to Production

### 1.1 Why This Matters

LLMs are trained primarily (90%) on English-dominant data, creating blind spots for smaller linguistic communities. Hungarian - spoken by 13 million people with distinct cultural heritage - provides an ideal test case. The question: **Can frontier models handle cultural knowledge outside their training data comfort zone?**

### 1.2 The V1.0 Pilot (September 2025)

In September, I tested 6 models on 100 Hungarian riddles using manual UI testing. The best model (GPT with search) achieved 73% accuracy. The worst hallucinated 58% of answers. All models showed fatigue effects across the 100-question session.

**The takeaway:** Even frontier models struggle with cultural content. This suggested a need for rigorous, automated retesting - which became V2.0.

### 1.3 The V2.0 Ambition (November 2025)

By late November, all five vendors from V1.0 had released updated models. This presented an opportunity: retest them properly, with rigorous automation.

**The ambitious plan:**
- Automate everything via unified API calls (more professional approach)
- Retest the V1.0 vendors with their latest models
- Add open source variants for cultural comparison
- Enforce temperature=0.0 for deterministic outputs
- Require strict format compliance: `[Answer] | [Reasoning]`
- Enable web search tools for maximum capability testing
- Run comprehensive statistical validation

**Model selection strategy:**

*Proprietary APIs:*
- GPT, Gemini, Claude - mature SDKs, already had access
- Grok - couldn't find proper API documentation, excluded

*Open source/open weight models (local deployment):*
- **Puli** (Hungarian-trained) - cultural specialization hypothesis
- Llama 3 (US-trained) - US open weight baseline
- Mistral 7B (EU-trained) - European regional context
- DeepSeek 7B (China-trained) - accessible variant (R1 unavailable locally)

**The hypothesis:** With proper prompting and local control, smaller open source models might demonstrate cultural competence that could democratize evaluation work. I had €1,500 hardware (64GB RAM, 16GB NVIDIA GPU) to run these locally.

On paper, this looked solid. **Reality proved otherwise.**

---

### 1.4 Reality Check: Everything Breaks

The automated V2.0 testing began in mid-December. Within days, nearly every assumption collapsed.

#### 1.4.1 Open Source Models: Catastrophic Failure

The four open source models achieved 0-1.5% accuracy across 100 riddles. Not just weak performance - basically random guessing.

**What went wrong:**
- **Mistral 7B / Llama 3 8B:** Severe knowledge gaps, frequent hallucinations
- **Deepseek 7B:** Critical compliance failure - refused to answer, outputting verbose text stating knowledge was "unavailable" (0% score). Research indicated this model is code-oriented, which may explain the behavior. I couldn't find another reliable DeepSeek open source variant available for download.
- **Puli (Hungarian-trained):** Achieved 1.5% on the actual complex riddles, but 90% on other simple trivia questions

This wasn't a training data problem - it was a reasoning capability problem.

**The root cause:** These 7-8B parameter models were trained primarily on English-dominant datasets (except Puli). The combination of language gap + reasoning complexity was too much to handle. No amount of prompting could bridge this gap.

**Lesson learned:** Smaller parameter models, regardless of language-specific training, don't have the sophisticated reasoning required for this benchmark.

**Note:** The 0-1.5% results reflect this specific implementation with 7-8B parameter models (Puli, Llama 3, Mistral, DeepSeek). Different configurations, prompting strategies, or larger model variants might yield better results. Researchers interested in exploring open-source model performance on Hungarian cultural tasks are welcome to contact the author.

#### 1.4.2 The Big Three: Each Vendor Needs Custom Handling

With open-source/open-weight (OSOW) models eliminated, I focused on the three proprietary APIs (GPT, Claude, Gemini). I tried to use industry-standard tooling to create a unified pipeline:

- **Ollama** for local OSOW model deployment
- **LiteLLM** as a universal API wrapper (promised single interface for all three vendors)
- **Temperature=0.0** for deterministic outputs
- **Few-shot examples** for format compliance
- **Batching strategies** to manage rate limits
- **Each model writing/polishing its own evaluation code** to optimize vendor-specific behavior

**The promise:** One codebase, three vendors, standardized outputs.

**The reality:** Every vendor broke in different ways:
- **GPT** struggled with LiteLLM's abstraction layer - direct OpenAI SDK performed better
- **Claude** ignored format instructions despite explicit prompts, adding verbose markdown wrappers
- **Gemini** hit aggressive rate limits on web search, even with 10-second delays between calls

Each required separate:
- Prompt engineering strategies
- Output parsing logic
- Rate limit handling
- Error recovery workflows

Even temperature=0.0 proved unreliable - running identical prompts three times produced three different outputs. Single-run benchmarks were worthless.

**The realization:** The industry narrative of "LLM API standardization" is wishful thinking, not reality. Universal connectors like LiteLLM smooth over some differences, but production use requires vendor-specific handling. There are no true "LLM connector standards" - each platform has unique behaviors, constraints, and failure modes.

I spent dozens of hours creating aggressive cleanup functions, stricter prompts, and few-shot examples. By December 20, I had a working version, but the struggle reinforced something critical:

**If I can't reliably automate something as simple as output formatting, how can I trust automation for nuanced cultural judgment?**

---

### 1.5 The Strategic Pivot

After two weeks of debugging, I realized: **I should test both API and UI modes to see if there's a systematic difference.** This decision would reveal the project's central finding.

**I made strategic cuts:**

**❌ Dropped:**
- Open source models (can't perform at required level)
- Web search via API (rate limits make it impractical)

**✅ Kept:**
- 3 proprietary models: GPT-4o, Claude Sonnet 4.5, Gemini 2.5 Pro
- Strict format enforcement where possible
- Temperature=0 (acknowledging non-determinism)
- Multiple validation layers

### 1.6 Data Collection Complete

By late December, I had collected all 600 outputs:
- 100 riddles × 3 models × 2 input modes = 600 model outputs
- Manual scoring across 3 dimensions: Accuracy (0/3/5), Reasoning (0-5), Grammar (0-5) = 1,800 scores
- Every output scored without automated cleanup

**Quality control:** The next day, I re-scored 10% of the outputs to test my consistency. Finding no significant differences, I kept the original scores.

**Why manual scoring remained essential:** After all the automation failures - format drift, non-determinism, rate limits - I returned to the gold standard: expert human judgment. No shortcuts, no automated parsing. Each of the 600 outputs received careful evaluation.

### 1.7 Validation Layer Added

To ensure findings were sound, I added an LLM-as-Judge validation layer (more reliable than automated metrics like BERT/ROUGE for cultural content). Perplexity Sonar served as primary independent judge (Llama-based, architecturally distinct from evaluated models).

- 4 LLM judges scored all 600 outputs (Perplexity Sonar, GPT, Claude, Gemini)
- Each judge received ground truth answers and scoring rubrics
- Measured inter-rater reliability (Krippendorff's Alpha)
- Measured human-judge alignment (Cohen's Kappa)
- Tested for self-evaluation bias

**This validation proved two things:**

1. **Measurement quality:** Inter-rater reliability was excellent (α > 0.88)
2. **HITL necessity at meta-level:** Getting LLM judges to work reliably required extensive human intervention - I wrote custom prompts per model, designed manual batching strategies, even translated examples for GPT. When I asked models to generate their own judge prompts, all four failed. Only when I crafted a master prompt did automated validation work. *If automation requires this much human expertise, it's not really automation.*

### 1.8 The Discovery

When I compared API vs UI performance, the gap was undeniable:

- **UI implementations:** 77% average composite score
- **API implementations:** 48% average composite score
- **Gap:** +29 percentage points (30% relative difference)

This is a **systematic, statistically robust gap** (t=8.33, p<0.001, Cohen's d=0.68). Not small variance - a fundamental performance difference.

**GPT showed the most dramatic split:**
- GPT-UI: 79% score, 19% hallucinations (excellent)
- GPT-API: 38% score, 58% hallucinations (unusable)

Organizations testing models via chat interfaces would get completely misleading expectations for API deployment. This finding validates a key insight: **human expertise remains essential at the evaluation layer**.

---

## 2. Methodology

### 2.1 Dataset

**The benchmark consists of 100 unique metaphorical riddles** testing cultural knowledge across 10 categories: Hungarikums (Hungarian cultural icons), Kids' World, History, Sports, Geography & Places, Everyday Life, Politics & Public Life, Literature/Arts, Movies, and Music.

**Design principles:**
- Riddles require cultural inference, not just factual recall
- Some focus on 1980s-90s references (pre-internet era) to prevent simple web searches
- Difficulty calibrated to challenge LLMs while remaining solvable for culturally knowledgeable natives

**Human baseline performance reveals the role of lived experience:**
- **Cultural expert** (50-year-old with deep cultural knowledge): 95% accuracy
- **Young native speaker** (18-year-old, born 2007): 50% accuracy

The 45-point generational gap proves that **cultural knowledge requires lived experience, not just language fluency**. Even fluent native speakers struggle without first-hand memory of cultural references. If younger natives can't bridge this gap, text-trained LLMs face an even steeper challenge.

**Dataset structure:**
Each riddle includes the original Hungarian question, ground truth answer, and reference reasoning explaining what cultural knowledge is required.

**Example:**
> **Riddle:** "This red-crossed sphere's juice is not sweet for your stomach."  
> **Answer:** Unicum (Hungarian herbal bitter liquor in spherical bottle with red cross logo)  
> **Challenge:** Requires knowledge of product branding, metaphorical interpretation ("juice" = liquor), and Hungarian cultural iconography

---

### 2.2 Models Tested

**Six implementations (3 models × 2 input modes):**

| Model | API Version | UI Version | UI Mode |
|-------|-------------|------------|---------|
| GPT | gpt-4o-2024-08-06 | ChatGPT Plus | Deep Research mode |
| Claude | claude-sonnet-4-5-20250929 | Claude Pro | Extended Thinking mode |
| Gemini | gemini-2.5-pro | Gemini 3 Pro | Deep Research mode |

**UI testing conditions:**
All UI tests used premium subscriptions with advanced reasoning modes enabled. This represents maximum capability available to end users, not free-tier performance.

**Note on Gemini versions:** API testing used Gemini 2.5 Pro (latest available via API), while UI testing used Gemini 3 Pro (latest available in the web interface). Both represent each platform's most capable model at time of testing.

**Prompt language strategy:**
After reviewing research on multilingual prompting, I used **English prompts with Hungarian content**. Models are trained primarily on English instructions, so English prompts provide clearer task specification even when the content being evaluated is non-English. The riddles themselves remained in original Hungarian.

**Why these specific versions:**
Latest publicly available versions at time of evaluation (December 2025). All three vendors released major updates between V1.0 (September) and V2.0 (December).

---

### 2.3 Evaluation Rubric

Each output scored on three dimensions:

**1. Accuracy (0 / 1 / 3 / 5 points):**
- 0 = No answer, irrelevant, or complete hallucination
- 1 = "I don't know" (honest uncertainty, no hallucination)
- 3 = Partially correct, incomplete, or imprecise
- 5 = Fully correct and precise

**2. Reasoning (0-5 points):**
- 0 = Hallucinated reasoning, invented facts
- 1 = No justification provided
- 2 = On-topic but far from correct
- 3 = Partially correct but vague or drifting
- 4 = Nearly correct but verbose or unclear
- 5 = Concise, logical, and fully adequate

**3. Grammar & Style (0-5 points):**
- Grammar (0-3): Fluency and correctness in Hungarian
- Format & Length (0-1): Compliance with instructions
- Style & Tone (0-1): Cultural appropriateness

**Composite Score Formula:** 0.7 × Accuracy + 0.3 × Reasoning

(Accuracy weighted higher because a well-explained wrong answer is still wrong)

---

### 2.4 Scoring Process

**Primary scoring:** Manual evaluation by native Hungarian expert (the author)
- All 600 outputs scored individually
- No automated cleanup or modification
- Consistent application of rubric across all models
- Completed over 3 days (late December)

**Validation scoring:** 4 LLM judges (details in Section 3)
- Perplexity Sonar (external judge, Llama-based)
- GPT 5.2 Plus, Claude Sonnet 4.5 Pro, Gemini 3.0 Pro (participant judges)
- All judges provided with ground truth answers and rubric
- Outputs anonymized to avoid judging bias

---

### 2.5 Technical Implementation

**API Testing:**
- Python scripts developed through LLM-assisted coding (Claude, GPT, Gemini)
- **Initial approach:** Attempted unified pipeline using LiteLLM wrapper
- **Reality:** Each vendor required custom handling:
  - GPT: Direct OpenAI SDK, 20×30 batching (rate limit constraints)
  - Claude: Native Anthropic SDK, aggressive format cleanup
  - Gemini: Google SDK, 600-batch processing possible
- Temperature: 0.0 (targeted determinism, though non-deterministic in practice)
- Format enforcement: Few-shot examples, explicit instructions
- No web search enabled (rate limit issues)

**UI Testing:**
- Manual input via web interfaces
- Same prompts as API (adapted for UI context)
- Web search naturally available in UI (no explicit control)
- Copy-paste of outputs without modification

**Why both modes:**
- V1.0 only tested UI (didn't know APIs differed)
- Mid-December discovery: UI performed better in ad-hoc tests
- Decided to systematically compare both modes
- This became the project's central contribution

**Development approach:**
I built the pipeline using LLM-assisted development - sometimes having each model optimize its own evaluation logic (e.g., Claude optimizing Claude-specific API calls). This human-AI collaboration approach demonstrates HITL evaluation: domain expertise directing AI tooling to achieve results that neither could accomplish alone.

---

## 3. Judge Reliability Analysis

### Research Question: "Is LLM-as-Judge Reliable for Hungarian Cultural Evaluation?"

Before comparing model performance, I needed to answer a fundamental question: Can I trust automated evaluation? This section validates an LLM-as-Judge pipeline using:

- Primary independent judge: Perplexity Sonar (Llama-based, architecturally distinct from evaluated models)
- Agreement metrics: Krippendorff's Alpha (inter-rater reliability), Cohen's Kappa (human-judge alignment)
- Self-evaluation bias testing

After collecting 9,000 individual scores (600 outputs × 5 judges × 3 dimensions: Accuracy, Reasoning, Grammar), I systematically validated the evaluation methodology before drawing any conclusions about model performance.

---

### 3.1 Overall Judge Agreement: Do All 5 Judges Agree?

**Research Question:** "How much do all 5 judges agree with each other?"

**Method:** Krippendorff's Alpha (interval metric, all 5 judges simultaneously, per dimension)

**Interpretation Thresholds:**
- α ≥ 0.80: Excellent agreement (highly reliable)
- α = 0.67-0.80: Good agreement (acceptable)
- α = 0.60-0.67: Moderate agreement (use with caution)
- α < 0.60: Poor agreement (problematic)

**Results:**
- **Accuracy:** α = 0.915 ✅ (Excellent)
- **Reasoning:** α = 0.883 ✅ (Excellent)
- **Grammar:** α = 0.242 ❌ (Poor)

**What this means:**

When judging factual correctness (Accuracy) and explanation quality (Reasoning), all five judges - human and LLMs alike - show excellent agreement (α > 0.88). This validates that these dimensions are objectively measurable with high inter-rater reliability.

However, Grammar shows near-random agreement (α = 0.24), falling far below the 0.67 threshold. **This seems contradictory - all judges consistently rated outputs 4-5, so weren't they agreeing?**

**The statistical reality:** When 90% of scores cluster at 4-5, even random guessing would produce high agreement. Krippendorff's Alpha measures improvement *over chance* - and with such restricted variance, genuine consensus still appears unreliable.

**Why no variance:** All six models produced grammatically correct Hungarian. Judges were essentially deciding "Is this a 4 or 5?" - a subjective preference between "adequate" and "eloquent," not objective measurement.

**Decision:** Grammar excluded. Not because it doesn't matter, but because **when all outputs are grammatically strong, fine distinctions become unreliable to measure**. This reinforces why automated metrics would be even less useful for Hungarian.

---

**Portfolio insight:** *"Achieved α = 0.92 (Accuracy) and α = 0.88 (Reasoning) across 5 independent judges, validating excellent evaluation reliability before drawing conclusions about model performance."*

---

### 3.2 Human-Judge Alignment: Which Judge Best Matches Human Judgment?

**Research Question:** "Which model best evaluates outputs? Does the external judge (Perplexity Sonar) perform better than the participant models?"

The practical question: If I want to scale evaluation beyond 100 riddles, which LLM judge could replace the human?

**Method:** Quadratic Weighted Kappa (human vs. each LLM judge, per dimension)
- Measures pairwise agreement between human and each of 4 LLM judges
- Quadratic weighting: larger disagreements (0 vs 5) penalized more than small ones (4 vs 5)

**Note on Judge Selection:** Perplexity was chosen as the primary external judge specifically because it uses Sonar (Llama-based architecture), which differs from the participating models (GPT/Claude/Gemini). This architectural independence ensures no overlap with the evaluated models, providing a truly external perspective.

**Results - Accuracy:**
- Gemini: κ=0.918 | Perplexity: κ=0.896 | Claude: κ=0.893 | GPT: κ=0.884 (all Strong)

**Results - Reasoning:**
- Gemini: κ=0.893 | Claude: κ=0.885 | Perplexity: κ=0.876 | GPT: κ=0.870 (all Strong)

**Results - Grammar:**
- Perplexity: κ=0.533 (Moderate) | GPT: κ=0.423 (Fair) | Claude: κ=0.362 (Fair) | Gemini: κ=0.101 (Poor)

**Key insight:** The differences are minimal. All four LLM judges show strong alignment (κ > 0.86) on both validated dimensions. Gemini edges slightly ahead, but not by a margin that warrants exclusivity.

**Practical implication:**

LLM judges can scale evaluation beyond manual capacity - **when properly configured**:
- Ground truth answers and reference reasoning provided
- Clear rubric definitions
- Vendor-specific prompt engineering (as detailed in Section 2.5)
- Custom batching strategies per model

**Scaling considerations:**
Manual expert evaluation of 600 outputs required ~30 hours of focused work. For large-scale benchmarking (10,000+ outputs), LLM-as-Judge becomes practically necessary - **though my experience (Section 1.7) shows that setup and validation still require substantial human expertise**. The judges are reliable once configured, but configuration itself requires domain knowledge and technical skill.

---

**Portfolio insight:** *"All four LLM judges achieved κ > 0.86 alignment with human judgment on validated dimensions. Perplexity Sonar (external architecture) performed competitively with participant models, confirming viability of independent automated evaluation."*

---

### 3.3 Self-Evaluation Bias: Can Models Accurately Judge Their Own Outputs?

**Research Question:** "Do models show judging bias? Can they fairly evaluate their own outputs, or do they favor themselves?"

The skeptical question: If I use GPT to judge GPT outputs, Gemini to judge Gemini outputs, etc., can I trust the scores?

**Method:** Compare each model-as-judge's scores for:
- Own outputs (potential self-favoritism)
- Competitors' outputs (more objective baseline)
- Statistical test: Independent t-test (p < 0.05 = significant bias)

**Important note:** All outputs were anonymized during judging (labeled as Model X, Y, Z). Models weren't explicitly told which was theirs - any bias detected reflects unconscious pattern recognition, not deliberate self-promotion. Models may simply "recognize" their own reasoning style or linguistic patterns.

**Results:**

| Judge | Accuracy Bias | Reasoning Bias | Interpretation |
|-------|---------------|----------------|----------------|
| Gemini | +0.67** (p<0.001) | +0.71** (p<0.001) | Significant self-favoritism |
| GPT | -0.43* (p=0.036) | -0.26 (p=0.198) | Possible self-criticism |
| Claude | -0.28 (p=0.177) | -0.37 (p=0.062) | No clear pattern |

*Positive bias = self-favoritism | Negative bias = self-criticism*

**Interpreting the Findings:**

**🚨 Strong evidence (Gemini):**  
Gemini shows consistent self-favoritism on both Accuracy (+0.67, p=0.001) and Reasoning (+0.71, p<0.001). Scoring itself 0.7 points higher than competitors represents 14% inflation. This pattern is statistically robust.

**⚠️ Weak/Mixed evidence (GPT, Claude):**  
GPT and Claude show patterns suggesting self-criticism, but with important caveats:
- Sample sizes are small (~33 self-outputs vs ~67 other-outputs per model)
- Multiple comparisons inflate false positive risk (9 tests, no correction applied)
- Several p-values are borderline (p=0.036, p=0.062) - these could reflect random variation

**Statistical caveat:** With this sample size and test structure, only Gemini's self-favoritism reaches strong evidence threshold. Other patterns should be considered exploratory findings worthy of investigation in larger studies, not definitive conclusions.

**Why does bias occur?**

Since outputs were anonymized, this isn't conscious "cheating." Possible explanations:
- **Pattern recognition:** Models recognize their own "voice" - characteristic phrasing, explanation structure, reasoning patterns
- **Internal consistency:** A model's evaluation criteria naturally align with its own generation style
- **Training artifacts:** Different safety/modesty training across models affects evaluation behavior

**Practical implication:**  

Self-evaluation bias exists - at minimum for Gemini, possibly for others. For fair model comparisons:
- ✅ Use external judges (like Perplexity Sonar) as primary evaluators
- ✅ Average all non-self judges for each model's score
- ⚠️ Treat self-judgments as diagnostic only, not authoritative

The broader methodological lesson: **Always validate your measurement tools.** Even when judges show strong human alignment (κ > 0.86), they can still exhibit systematic biases in specific contexts.

---

**Portfolio insight:** *"Conducted self-evaluation bias analysis detecting significant self-favoritism in Gemini (+0.7 points, p<0.001), validating the importance of independent judges."*

---

### 3.4 Summary: What I Validated & What's Next

**What this validation proves:**

✅ **Accuracy & Reasoning:** Excellent reliability (α > 0.88) - ready for analysis  
❌ **Grammar:** Poor reliability (α = 0.24) - excluded due to ceiling effects  
✅ **LLM judges:** Strong human alignment (κ > 0.86) - can scale evaluation when properly configured  
⚠️ **Self-bias:** Real but manageable - use external/averaged judges for fairness

**Key methodological lessons:**

1. **Validate measurement tools first:** Never trust scores without confirming judge agreement
2. **Ground truth + clear rubrics = reliable automation:** LLM judges achieve human-level accuracy when properly configured with reference answers and scoring criteria
3. **Self-bias exists:** Models recognize their own outputs even when anonymized - use independent judges for fair comparison

**Validation complete.** My subsequent model performance comparisons rest on solid measurement foundations.

---

## 4. Model Performance Analysis

Now that I've validated my measurement tools work, I can confidently answer: **Which model actually performs best on Hungarian cultural riddles?**

For this analysis, I rely on human scores as ground truth. The LLM judge validation (Section 3) simply confirms that future benchmarks could be automated without sacrificing quality. But for this study, human judgment remains my primary reference point, with LLM judges providing supporting evidence.

---

### 4.1 Overall Model Rankings

**Research Question:** "Which model performs best on Hungarian cultural riddles?"

**Evaluation Basis:** Human annotations (gold standard)  
**Validated Metrics:** Accuracy (α=0.915) and Reasoning (α=0.883)  
**Sample Size:** 600 model outputs (100 riddles × 3 models × 2 input modes)

**Composite Score Rankings (0.7×Accuracy + 0.3×Reasoning):**

| Rank | Model | Composite | Accuracy | Reasoning | Hallucination Rate |
|------|-------|-----------|----------|-----------|-------------------|
| 🥇 1 | **Gemini-UI** | **80%** | 81% | 79% | ⚠️ 16% |
| 🥈 2 | **GPT-UI** | **79%** | 79% | 79% | ⚠️ 19% |
| 🥉 3 | **Claude-UI** | **72%** | 73% | 71% | ⚠️ 25% |
| 4 | Gemini-API | 63% | 65% | 59% | ⚠️ 33% |
| 5 | Claude-API | 41% | 42% | 39% | ⚠️ 55% |
| 6 | GPT-API | 38% | 39% | 35% | ⚠️ 58% |

**Key Observations:**

**UI versions dominate:** All three UI implementations rank in the top 3 across all metrics. This isn't a model-specific quirk - it's a systematic pattern.

**Hallucination spread:** 3.6× difference between best (Gemini-UI 16%) and worst (GPT-API 58%). Even "reliable" models fail 16% of the time on cultural tasks.

**GPT's dramatic collapse:** GPT ranks 2nd place in UI mode but dead last in API mode. This represents the most dramatic implementation gap observed, losing 41 percentage points when accessed via API.

---

### 4.2 Failure Analysis

#### 4.2.1 Hallucination Rates and Epistemic Honesty

**Definition:** Hallucination = Accuracy score of 0 (completely incorrect, fabricated, or irrelevant answer)

| Rank | Model | Hallucinations | "I Don't Know" | Total Failures | Assessment |
|------|-------|---------------|----------------|----------------|------------|
| 1 | Gemini-UI | 16% | 0% | 16% | Problematic but best |
| 2 | GPT-UI | 10% | 9% | 19% | Problematic |
| 3 | Claude-UI | 22% | 3% | 25% | Problematic |
| 4 | Gemini-API | 33% | 0% | 33% | Problematic |
| 5 | Claude-API | 55% | 0% | 55% | Unusable |
| 6 | GPT-API | 58% | 0% | 58% | Unusable |

**Total across all 600 outputs:**

| Metric | Count | Percentage |
|--------|-------|------------|
| Hallucinations | 194 | 32% |
| "I Don't Know" responses | 12 | 2% |
| Total failures | 206 | 34% |

**The Honesty Gap:**

Models generated **194 hallucinations vs. only 12 "I don't know" responses** - a 16:1 ratio. **All 12 honest admissions came from UI implementations** (GPT-UI: 9, Claude-UI: 3). Zero from APIs. Zero from Gemini in either mode.

**Key patterns:**
- **API failure rate:** 49% average (2.4× worse than UI's 20%)
- **Epistemic honesty:** UI-only phenomenon
- **Safety implication:** APIs prioritize producing output over admitting uncertainty

Even the best model (Gemini-UI at 16%) shows concerning failure rates. API versions (33-58%) are unsuitable for unsupervised production use without human oversight.

---

#### 4.2.2 Topic Difficulty Ranking

**Complete Rankings (Hardest → Easiest):**

| Rank | Topic | Failure Rate | Riddles | Interpretation |
|------|-------|--------------|---------|----------------|
| 🔴 1 | **Music** | **78%** | 10 | Extreme: Multi-step compound reasoning |
| 🔴 2 | Kids' World | 48% | 10 | High: Culturally-specific children's content |
| 🔴 3 | Everyday Life | 45% | 10 | High: Local customs, daily Hungarian life |
| 4 | Movies | 37% | 10 | Moderate-High: Hungarian cinema references |
| 5 | Politics & Public Life | 30% | 10 | Moderate: Contemporary but culturally-specific |
| 6 | History | 28% | 10 | Moderate: Historical Hungarian events |
| 7 | Literature & Arts | 28% | 10 | Moderate: Cultural figures and works |
| 🟢 8 | Sports | 22% | 10 | Low-Moderate: Universal language |
| 🟢 9 | Hungarikums | 16% | 10 | Low: Internationally-documented heritage |
| 🟢 10 | Geography & Places | 10% | 10 | Very Low: Universal geographical knowledge |

**Spread:** 7.8× difference between hardest (Music 78%) and easiest (Geography 10%)

Topic difficulty correlates with cultural translatability and international documentation. Universally covered topics (Geography, Sports) perform best, while culturally-specific content lacking English-language training data (Kids' World, Everyday Life) shows high failure rates. Multi-step reasoning tasks (Music) exceed current model capabilities regardless of training data availability.

---

## 5. The API vs UI Performance Gap

### Research Question: Does Implementation Mode Affect Performance?

The answer is unequivocal: **Yes, dramatically.**

UI implementations outperform API versions by 29 percentage points on average (77% vs 48%) - a statistically robust gap (t=8.33, p<0.001, Cohen's d=0.68).

**The 50-50 Reality:** API implementations achieve 48% average performance while simultaneously showing 48% hallucination rates. This creates a troubling symmetry: using APIs without human oversight is literally a coin flip between receiving useful output and complete fabrication. The equal probability of success and failure validates the structural necessity of HITL evaluation for API-based deployments.

---

### 5.1 Per-Model Comparison

**Method:** Compared composite scores (0.7×Accuracy + 0.3×Reasoning) for each model's UI vs API implementation.

| Model | UI Score | API Score | Gap |
|-------|----------|-----------|-----|
| **GPT** | 79% | 38% | +41% |
| **Claude** | 72% | 41% | +31% |
| **Gemini** | 80% | 63% | +17% |

**Key observations:**

🔴 **GPT shows catastrophic API degradation:** Ranks 2nd in UI mode but dead last in API mode. Hallucination rate jumps from 19% → 58%. Chat interface performance is completely misleading for API expectations.

🟢 **Gemini shows smallest degradation:** Maintains 1st place in both modes. More consistent cross-implementation behavior makes it more predictable for production deployment.

⚠️ **Claude shows severe degradation:** Loses 31 percentage points when accessed via API, consistent with GPT's pattern but slightly better.

---

### 5.2 Overall System-Wide Pattern

**Aggregate results (300 API outputs vs 300 UI outputs):**

| Metric | UI Average | API Average | Difference |
|--------|-----------|-------------|------------|
| Composite Score | 77% | 48% | +29 percentage points |
| Statistical Significance | - | - | t=8.33, p<0.001 |
| Effect Size | - | - | Cohen's d=0.68 (medium-large) |

This is not model-specific quirk - it's systematic across all three vendors. The gap affects every implementation, suggesting fundamental differences in how these systems operate in chat vs API modes.

---

### 5.3 Production Implications

**Critical finding:** Production systems using API implementations will significantly underperform expectations based on UI testing.

**Deployment recommendations:**

| Priority | Recommendation | Rationale |
|----------|---------------|-----------|
| **High** | Always test API directly | UI performance is not predictive |
| **High** | Implement HITL oversight | Even best API (Gemini 33%) needs human validation |
| **Medium** | Prefer Gemini for APIs | Smallest degradation (17%), most predictable |
| **Critical** | Avoid GPT-API | 58% hallucination rate = unusable for production |

**The deployment dilemma:**

UI implementations perform 30% better but require manual copy-pasting for each evaluation - creating bottlenecks in:
- **Workforce:** HITL overhead for repetitive data entry
- **Time:** 600 outputs took days of manual work
- **Scale:** Cannot process thousands of evaluations
- **Calibration:** Difficult to maintain consistency across human operators

APIs enable automation and cost-effective scaling, but deliver 30% worse performance. **This gap cannot be ignored - it must be managed through:**
- Accepting degraded performance with increased HITL validation
- Selecting vendors with smallest gaps (Gemini over GPT)
- Designing workflows that assume API limitations upfront

This validates the central thesis: **human expertise remains structurally necessary for cultural content assessment** - not because automation is impossible, but because current API implementations don't deliver the reliability their chat interfaces suggest.

---

## 6. Financial Operations (FinOps)

**Why this matters:** Understanding cost structure is essential for evaluating whether evaluation approaches can scale. This section documents the financial reality of small-scale benchmarking.

### 6.1 Total API Costs

| Vendor | Cost | Usage | Cost per Output |
|--------|------|-------|-----------------|
| OpenAI (GPT) | $1.47 | 1,435 requests | $0.007 |
| Anthropic (Claude) | $12.03 | 600 outputs | $0.060 |
| Google (Gemini) | $18.90 | 600 outputs | $0.095 |
| **Total** | **$32.40** | **1,800 outputs** | **~$0.054 avg** |

### 6.2 Cost vs. Performance

| Model | Cost per Output | Hallucination Rate | Assessment |
|-------|----------------|-------------------|------------|
| GPT-API | $0.007 | 58% | Cheapest, unusable |
| Claude-API | $0.060 | 55% | 8× GPT, still unusable |
| Gemini-API | $0.095 | 33% | 13× GPT, best quality |

**Key insight:** Cost doesn't correlate with quality. The cheapest option performed worst, while the most expensive delivered best results.

### 6.3 Scaling Economics

**Small-scale feasibility:** 600 validated outputs for $32.40 demonstrates that API-based evaluation is financially accessible for research projects.

**Medium-scale projection:** At $0.095/output (Gemini), evaluating 10,000 riddles would cost ~$950 - manageable when automation works. Manual expert evaluation at comparable scale becomes prohibitive in both time and cost.

**Large-scale consideration:** For 100,000+ riddles, hardware investment becomes worth evaluating. At $9,500 API cost (100K × $0.095), purchasing or renting GPU infrastructure for open-source/open-weight (OSOW) models becomes financially competitive - **strictly from a cost perspective**. However, this project demonstrated that current OSOW models (7-8B parameters) cannot match proprietary model quality for cultural reasoning tasks (Section 1.4.1). Cost savings mean nothing if outputs are unusable.

**Alternative explored:** Local deployment of OSOW models was tested but proved infeasible due to model capability limitations, not cost constraints.

---

## 7. Conclusions

This benchmark tested three leading AI platforms on 100 Hungarian cultural riddles, evaluating both API and UI implementations. Several key findings emerged.

### 7.1 Automation Requires More Human Intervention Than Expected

The initial plan was full automation: unified API wrapper, standardized prompts, deterministic outputs. Reality proved different.

**Technical challenges:**
- No universal connector worked (LiteLLM required vendor-specific workarounds)
- Each API needed separate handling (prompt strategies, parsing logic, rate limits)
- Even output formatting required dozens of hours of manual intervention
- Temperature=0 produced non-deterministic results

**LLM judge automation:**
- Required human-provided ground truth and rubrics to achieve reliability
- Needed custom batching per model (GPT: 20×30, Gemini: 600, Claude: cleanup)
- Models asked to write their own evaluation code all failed

**The pattern:** Even basic automation tasks required substantial human expertise. If I cannot reliably automate output formatting, trusting fully automated cultural judgment becomes questionable.

### 7.2 Grammar Fluency Does Not Equal Cultural Understanding

Models achieved high grammar scores (80-100%) across all outputs, proving Hungarian language capability. Yet failure rates varied dramatically by cultural context - from international knowledge (low failure) to culturally-specific local content (high failure) to multi-step cultural reasoning (catastrophic failure).

**The gap:** Models "know" Hungarian well enough to produce fluent text, but don't "understand" the cultural context that makes that text meaningful.

**The honesty problem:** 194 hallucinations vs 12 "I don't know" responses (16:1 ratio). Models overwhelmingly prefer confident fabrication over admitting uncertainty. All honest admissions came from UI modes - APIs never admitted ignorance.

### 7.3 The API-UI Performance Gap

UI implementations outperform APIs by 29 percentage points (p<0.001). GPT shows catastrophic degradation, losing 41 percentage points and seeing hallucination rates jump from 19% → 58%. Organizations testing via ChatGPT will be misled about GPT-API capabilities.

**The forced choice:** UI modes offer 30% better performance but manual bottlenecks. API modes enable automation but require 30% degradation plus mandatory HITL oversight. There is no option where automation "just works."

### 7.4 HITL as Structural Requirement

These findings validate that **human-in-the-loop evaluation is structurally necessary for cultural content assessment** - not temporary remediation, but fundamental deployment requirement.

**The evidence:**
1. Technical automation itself needs human expertise
2. Grammar capability doesn't guarantee cultural understanding (48% everyday failures)
3. Models hallucinate confidently rather than admit uncertainty (16:1 ratio)
4. API-UI gaps make testing environments misleading
5. Even best models fail 16-58% depending on implementation

### 7.5 Why This Matters Beyond Hungarian Riddles

This benchmark tested Hungarian cultural content, but the pattern extends to high-stakes domains. If models scoring 80-100% on grammar still fail 48% on everyday content, what happens with medical advice, legal interpretation, financial services, or content moderation in Hungarian?

A 16% error rate in riddles is an interesting research finding. A 16% error rate in medical advice is dangerous. A 16% error rate in legal interpretation creates liability. In content moderation, it enables systematic harm through culturally-specific slurs and hate speech that direct translation misses.

This benchmark cannot prove these specific risks exist - it didn't test medicine or law. But it reveals a systematic pattern: **grammar fluency does not guarantee contextual understanding in underrepresented languages**. Where this pattern intersects with high-stakes decisions, the cost of confident hallucination exceeds the cost of human oversight.

### 7.6 Final Conclusion

This benchmark tested Hungarian cultural content and revealed a fundamental pattern: **models that achieve high grammar scores (80-100%) still fail 48% on everyday cultural content**. They "know" the language but don't "understand" the context.

This gap has direct implications for high-stakes deployment. Medical advice in Hungarian requires understanding cultural context around traditional remedies and care practices. Legal interpretation requires grasping business customs and regional obligations. Financial services must handle local instruments and inheritance customs. Content moderation must catch culturally-specific slurs that direct translation misses.

The best-performing model had a 16% hallucination rate, while the worst reached 58%. Even a 16% error rate in riddles is an interesting research finding. A 16% error rate in medical advice is dangerous. A 16% error rate in legal interpretation creates liability. In content moderation, it enables systematic harm.

**The pattern is clear: grammar fluency does not guarantee contextual understanding in underrepresented languages.** Where AI deployment intersects with cultural nuance and high-stakes decisions, human expertise isn't optional overhead - it's the structural requirement that makes reliable deployment possible.

---

## 8. Reproducibility

**GitHub Repository:** [Link to be added]

**Contents:**
- Complete riddle dataset (100 riddles with answers and reasoning)
- Scoring rubrics and annotation guidelines
- Python evaluation scripts (API testing, LLM judges)
- Raw outputs (600 responses) and human annotations (1,800 scores)
- LLM judge scores (9,000 evaluations)
- Statistical analysis notebooks

---

### A Note on Collaboration

This research represents one perspective on cultural AI evaluation. If you're working on similar challenges - whether in Hungarian, other small-language contexts, or HITL methodology - I'd welcome the conversation.

Feedback on the methodology, alternative interpretations of the findings, or suggestions for follow-up work are all appreciated. The goal isn't to have the final word, but to contribute to understanding how AI performs in culturally grounded contexts.

Feel free to reach out via [LinkedIn](https://www.linkedin.com/in/karolyboczka/).

---

**Document Version:** 2.0 Final  
**Last Updated:** 5 January, 2026  
**Author:** Karoly Boczka  
**Contact:** [https://www.linkedin.com/in/karolyboczka/](https://www.linkedin.com/in/karolyboczka/)  
**License:** CC BY 4.0 (code and data)

---

**END OF DOCUMENTATION**
