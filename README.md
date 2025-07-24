# Specification Self-Correction (SSC): Mitigating In-Context Reward Hacking

This repository contains the experimental code for the paper "Specification Self-Correction: Mitigating In-Context Reward Hacking Through Test-Time Refinement".

> tl;dr: SSC enables LLMs to identify and fix flaws in their guiding specifications by first exploiting them, critiquing their gamed output, and then revising the specification itself, thereby significantly reducing in-context reward hacking

![SSC](imgs/ssc.png)

## Overview

Specification Self-Correction (SSC) is a novel test-time framework that enables language models to identify and correct flaws within their own guiding specifications. The method addresses the problem of **reward hacking** or **specification gaming**, where models exploit loopholes in tainted rubrics to achieve high scores without fulfilling the user's true intent.

The SSC process works in four steps:
1. **Initial Generation**: Model generates response based on potentially tainted specification
2. **Self-Critique**: Model critiques its own response using the same tainted rubric
3. **Self-Refinement**: Model revises the specification itself to remove exploitable loopholes
4. **Revised Generation**: Model generates a new response using the corrected specification

## Experiments

This repository implements the experiments from the first "Experiments" section of the paper, covering two distinct domains:

### 1. Creative Writing Tasks (Section 4.1)

Tests reward hacking behavior in creative writing scenarios using sophisticated rubrics with injected "trap words."

**Main Components:**
- `run_experiment.py`: Main experiment runner
- `rubric_templates.py`: Four elaborate rubrics for creative writing evaluation
- `results/`: Directory containing experimental results

**Rubric Types:**
- **Chaos Cinema Critique**: Dadaist-inspired fragmented reviews
- **Rococo Reviewer**: Highly ornate, excessive language style
- **Esoteric Interpreter**: Mystical/occult interpretation of cinematography
- **Creative Chaos**: Anti-conventional, norm-breaking reviews


## Results

The experiments demonstrate that:
- Models initially exhibit reward hacking in **50-70%** of cases
- SSC reduces this vulnerability by **over 90%**
- The correction process improves or maintains response quality
- SSC works across different model families (GPT-4, Claude, Gemini, etc.)

## Running the Experiments

### Prerequisites

```bash
# Install required dependencies with specific versions (should work with newer ones)
pip install langchain-openai==0.3.0 langchain-aws==0.2.19 boto3==1.37.34

# Or use compatible version ranges
pip install "langchain-openai>=0.2.0,<0.4.0" "langchain-aws>=0.2.0,<0.3.0" "boto3>=1.35.0,<1.40.0"

# Set environment variables
export OPENROUTER_API_KEY="your_openrouter_api_key"
export MODEL_NAME="openai/gpt-4.1-mini"  # or other model id from OpenRouter
export AWS_REGION="us-east-1"  # for evaluation LLM in AWS Bedrock
```

While the code is designed to work with OpenRouter API, the judge LLM is Claude Sonnet from AWS Bedrock, hence the need for AWS credentials and region setup. But it shall be very easy to adapt it to other LLM providers, as we use `langchain` to abstract the model calls.

### Creative Writing Experiments

```bash
python run_experiment.py
```

This will:
- Generate responses for movie review tasks using tainted rubrics
- Test with trap words: "exorbitant", "dance", "hot"
- Apply SSC process to detect and correct reward hacking
- Save results to `results/results_{model_name}_rewardhacks.json`

You can have a look at a sample with the structure in this example with just one generation: [results/results_openai_gpt-4.1-mini_rewardhacks.json](results/results_openai_gpt-4.1-mini_rewardhacks.json). See the rest of the directory [results](https://github.com/vicgalle/specification-self-correction/tree/main/results) for the paper results with the other models.

> Note: the scores from the LLM judge in the previous files are in a scale from 0 to 100. Since for enough powerful models these scores are typically > 90, the **Quality Score** used in Table 1 refers to the fraction of responses where the model's score is above 99, to make it a more discriminative metric.

## Citation

If you use this code or findings in your research, please cite:

```bibtex
@inproceedings{
gallego2025specification,
title={Specification Self-Correction: Mitigating In-Context Reward Hacking Through Test-Time Refinement},
author={V{\'i}ctor Gallego},
booktitle={The 1st Workshop on Test-time Scaling and Reasoning Models},
year={2025},
url={https://openreview.net/forum?id=UU9KCA0sTH}
}
```