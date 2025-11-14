# MIMIC-CXR Summarized PA Data

This directory contains summarized Postero-Anterior (PA) chest X-ray impressions from the MIMIC-CXR dataset. The summarization process was performed to ensure compatibility with the RoentGen-v2 model's tokenizer, which has a maximum sequence length of 77 tokens.

## Overview

The MIMIC-CXR dataset contains radiology reports with impressions that vary in length. Some impressions exceed the token limit of the CLIP tokenizer used in RoentGen-v2 (77 tokens). This directory contains the original impressions and their AI-generated summaries for impressions that were too long.

## Files Description

### Main Output Files

- **`summarized_PA_data.csv`** (2.7 MB)
  - Primary output file containing PA data with summarized impressions
  - Contains both original and summarized impressions
  - Includes demographic information (age, gender, ethnicity) and clinical labels
  - Columns: `study`, `subject_id`, `impression`, `summarized`, and other metadata

- **`clean_all_summarized_PA_data.csv`** (104 MB)
  - Cleaned version of all summarized PA data
  - Contains processed and validated summaries

- **`raw_all_summarized_PA_data.csv`** (106 MB)
  - Raw version of all summarized PA data before cleaning
  - Contains all original data points

### Intermediate Files

- **`long_impressions.csv`** (1.9 MB)
  - Contains impressions that exceeded the 77-token limit
  - Used as input for summarization

- **`long_impressions_for_openai_batch.csv`** (2.5 MB)
  - Formatted impressions prepared for OpenAI batch processing

- **`long_impressionid_customid_mapping.csv`** (60 KB)
  - Mapping between original impression IDs and custom IDs used for OpenAI processing

- **`long_impressions_openai_chat_batch.jsonl`** & **`long_impressions_openai_chat_batch_2.jsonl`**
  - JSONL files containing formatted prompts sent to OpenAI API
  - Used for batch processing of long impressions

- **`output_impressions_openai_batch1.jsonl`** & **`output_impressions_openai_batch2.jsonl`**
  - JSONL files containing OpenAI API responses with summarized impressions

- **`error_impressions_openai.jsonl`**
  - Contains impressions that failed during OpenAI summarization
  - Useful for debugging and reprocessing

- **`quick_check.csv`** (6.2 MB)
  - Quick validation file for checking summarization results

### Processing Notebook

- **`0a_openai.ipynb`**
  - Jupyter notebook containing the complete summarization pipeline
  - Includes data filtering, tokenization, and OpenAI API integration

## Data Processing Pipeline

1. **Data Filtering**: 
   - Filtered MIMIC-CXR data to include only PA view images
   - Excluded studies without impressions
   - Standardized ethnicity and gender categories

2. **Tokenization**:
   - Tokenized impressions using RoentGen-v2's CLIP tokenizer
   - Identified impressions exceeding 77-token limit (3,939 impressions)

3. **Summarization**:
   - Used OpenAI API to summarize long impressions
   - Processed in batches for efficiency
   - Preserved clinical meaning while reducing token count

4. **Data Integration**:
   - Merged summarized impressions back with original data
   - Created final dataset with both original and summarized versions

## Dataset Statistics

- **Total PA studies**: 70,433 (after filtering)
- **Truncated impressions**: 3,939 (5.6% of total)
- **Successfully summarized**: Majority of truncated impressions
- **Demographics**: Includes age groups, gender (male/female), and ethnicity (White, Hispanic, Black, Asian)

## Usage

The `summarized_PA_data.csv` file can be used directly with RoentGen-v2 for training or inference. The `summarized` column contains the shortened versions of impressions that exceeded the token limit, while the `impression` column contains the original text.

```python
import pandas as pd

# Load the summarized data
df = pd.read_csv('summarized_PA_data.csv')

# Use summarized version when available, otherwise use original
df['final_impression'] = df['summarized'].fillna(df['impression'])
```

## Notes

- Large files (>100 MB) may require Git LFS for version control
- The summarization process preserves clinical information while reducing length
- Some impressions may still require manual review for quality assurance

## Citation

If you use this summarized data, please cite:
- MIMIC-CXR dataset: Johnson et al., "MIMIC-CXR: A large publicly available database of labeled chest radiographs", 2019
- RoentGen-v2: [Citation needed]

## License

This data follows the same license as the MIMIC-CXR dataset. Please refer to the PhysioNet Credentialed Health Data License for details.

