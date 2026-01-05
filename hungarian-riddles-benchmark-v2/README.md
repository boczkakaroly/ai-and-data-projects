# Hungarian Cultural Benchmark v2.0

Testing LLM cultural understanding beyond English through 100 original Hungarian cultural riddles.

## Key Findings

- **API implementations:** 48% accuracy with 48% hallucination rate (literally a coin flip)
- **UI-API gap:** Chat interfaces outperform APIs by 30 percentage points on average
- **Honesty problem:** Models fabricated answers 16× more often than admitting "I don't know"
- **Grammar-understanding gap:** 80-100% grammar scores, 48% cultural accuracy

Models speak Hungarian fluently. They don't understand what they're saying.

## Contents

- `[Notebooks]/` - Python evaluation scripts and statistical analysis
- `[Prompts]/` - Prompt templates and configurations used for testing
- `Questions_Answers_Scores.xlsx` - Complete dataset: 100 riddles, 600 model responses, 10,800 scores (1,800 manual + 9,000 LLM-as-judge)
- `Riddles_Dashboard_v2.jpg` - Power BI dashboard visualization
- `Riddles_100.pbix` - Power BI source file for interactive exploration
- **Technical Documentation:** [Full methodology and results](./Hungarian_Riddle_Benchmark_v2.md)

## Reproducibility

All data, code, and scoring rubrics are included for independent validation.

**Author:** Károly Boczka  
**License:** CC BY 4.0
