# MIMIC-CXR Summarized PA Data

This directory contains summarized Postero-Anterior (PA) chest X-ray impressions from the MIMIC-CXR dataset. The summarization process was performed to ensure compatibility with the CLIP tokenizer, which has a maximum sequence length of 77 tokens.

## Overview

The MIMIC-CXR dataset contains radiology reports with impressions that vary in length. Some impressions exceed the token limit of the CLIP tokenizer (77 tokens). This directory contains the original impressions and their AI-generated summaries for impressions that were too long.

The summarization was performed using GPT-5 , based on prior work demonstrating that GPT-5 is highly capable in summarizing radiology reports [[1,2]](#references).

## Files Description

### Main Output Files

- **`summarized_PA_data.csv`** (2.7 MB)
  - Primary output file containing PA data with summarized impressions
  - Contains both original and summarized impressions
  - Columns: `study`, `subject_id`, `impression`, `summarized`

## Data Processing Pipeline

1. **Data Filtering**: 
   - Filtered MIMIC-CXR data to include only PA view images
   - Excluded studies without impressions
   - Standardized ethnicity and gender categories

2. **Tokenization**:
   - Tokenized impressions using CLIP tokenizer
   - Identified impressions exceeding 77-token limit (3,939 impressions)

3. **Summarization**:
   - Used GPT-5 [[38]](#references) to summarize long impressions
   - GPT-5 prompt: *"Your task is to summarize this radiology report within 200 characters or less. Your response must be concise, truthful, and keep all relevant medical information."*
   - Processed in batches for efficiency
   - Preserved clinical meaning while reducing token count
   - Based on prior work demonstrating GPT-5's capability in summarizing radiology reports [[1,2]](#references)
   - Total API cost: approximately $60 USD for batch processing all impressions

  
## Dataset Statistics

- **Total PA studies**: 70,433 (after filtering)
- **Truncated impressions**: 3,939 (5.6% of total)

## Usage

The `summarized_PA_data.csv` file can be used directly. The `summarized` column contains the shortened versions of impressions that exceeded the token limit, while the `impression` column contains the original text.

```python
import pandas as pd

# Load the summarized data
df = pd.read_csv('summarized_PA_data.csv')

# Use summarized version when available, otherwise use original
df['final_impression'] = df['summarized'].fillna(df['impression'])
```


## Citation

If you use this summarized data, please cite:
- MIMIC-CXR dataset: Johnson et al., "MIMIC-CXR: A large publicly available database of labeled chest radiographs", 2019

## References

[1] RadAdapt: Radiology Report Summarization via Lightweight Domain Adaptation of Large Language Models, ACL Anthology 2023 BioNLP Workshop. [Link](https://aclanthology.org/2023.bionlp-1.42/)  
[2] Adapted large language models can outperform medical experts in clinical text summarization, Nature Medicine 2024. [Link](https://www.nature.com/articles/s41591-024-02855-5)


## License

This data follows the same license as the MIMIC-CXR dataset. Please refer to the PhysioNet Credentialed Health Data License for details.

