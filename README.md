# GAIA Translation Evaluation

Evaluation framework for GAIA ML translation API performance on Aguaruna-Spanish translation using the Bouquet dataset.

## Overview

This project evaluates GAIA's translation quality from Aguaruna (awajun) to Spanish using multiple metrics:
- **BERTScore**: Semantic similarity evaluation
- **COMET**: Neural-based translation quality assessment  
- **chrF**: Character-level F-score

## Dataset

- **Source**: Facebook Bouquet dataset (`agr_Latn` → `spa_Latn`)
- **Sample size**: 200 randomly selected sentences
- **Seed**: 45 (for reproducibility)

## Requirements

```bash
pip install datasets huggingface_hub bert-score unbabel-comet sacrebleu
```

## Setup

1. Get your GAIA API key from https://gaia-ml.com/
2. Login to Hugging Face: `huggingface-cli login`
3. Update the `api_key` variable in the script

## Usage

The evaluation runs in three stages:

### 1. Translation
Translates Aguaruna sentences to Spanish using GAIA API with streaming response handling.

### 2. BERTScore Evaluation
Compares GAIA translations against reference translations using semantic similarity.

### 3. COMET & chrF Evaluation
Applies additional quality metrics for comprehensive assessment.

## Results

| Metric | Average | Min | Max | Std Dev |
|--------|---------|-----|-----|---------|
| **BERTScore F1** | 0.7618 | 0.6416 | 0.9769 | 0.0636 |
| **COMET** | 0.5973 | 0.3471 | 0.9405 | 0.1250 |
| **chrF** | 29.4870 | 6.3199 | 79.1521 | 13.9762 |

**Correlation**: COMET vs BERTScore F1 = 0.7502

## Output Files

- `bouquet_translation_results.json`: Raw translation pairs
- `bertscore_evaluation_results.json`: BERTScore analysis
- `combined_evaluation_results.json`: COMET + BERTScore results
- `final_evaluation_results.json`: Complete evaluation with all metrics

## Model Configuration

- **Model ID**: `d3936949-e2a3-4acc-b323-1050aafb2738`
- **API Endpoint**: `https://latest.gaia-ml.com/api/translate/`
- **Timeout**: 180 seconds per request
- **Batch size**: 8 (for COMET evaluation)

## Error Handling

The script includes robust error handling for:
- API timeouts and failures
- JSON parsing errors in streaming responses
- Missing reference translations
- Network connectivity issues
