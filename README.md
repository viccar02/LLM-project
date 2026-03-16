# LLM Project: Topic Extraction and Network Analysis of EU Parliament Debates

This project analyzes European Parliament debates using a large language model (LLM) pipeline to extract the main topic of each speech and then study topic patterns across political parties, years, and speakers. The workflow combines dataset filtering, structured LLM-based topic extraction, descriptive exploration, and bipartite network analysis. [file:2]

## Group Members

- Timo Philipse
- Victor Carmona
- Ioannis Chatzikos
- Kristjana Prifti
- Alvaro Buendia [file:2]

## Course Information

- **Course:** M2 – Natural Language Processing
- **Assignment:** Final NLP Project
- **Date:** 14/10/2025 [file:2]

## Project Overview

The notebook uses the `RJuro/eu_debates` dataset from Hugging Face and focuses on speeches delivered in Spanish. It filters the data to the years 2020, 2021, and 2022, removes entries without a party label, and samples 1,000 speeches to make LLM extraction manageable. [file:2]

After preprocessing, the project sends each translated speech to an LLM and asks for one short structured topic label. The extracted topics are saved to a CSV file and then used for downstream analysis, including topic frequencies, qualitative spot checks, and a speaker–topic graph. [file:2]

## Dataset

The notebook loads the `RJuro/eu_debates` training split and converts a filtered subset into a pandas DataFrame. The working sample contains 1,000 speeches and includes columns such as speaker name, role, party, language, date, year, debate title, original text, and translated text. [file:2]

The sampled speeches are distributed across the selected years as follows:
- 2020: 259 speeches
- 2021: 354 speeches
- 2022: 387 speeches [file:2]

Party representation in the sample is led by S&D, followed by PPE, ALDE, GUE/NGL, ECR, Greens/EFA, and NI. The notebook prints these party counts directly as part of the exploratory analysis. [file:2]

## Methodology

### 1. Data Filtering

The preprocessing pipeline applies the following steps:
- Keep only speeches with `intervention_language == "es"`.
- Restrict the dataset to the years 2020, 2021, and 2022.
- Remove entries where `speaker_party` is missing or marked as `"N/A"`.
- Randomly sample 1,000 speeches with a fixed seed for reproducibility. [file:2]

### 2. LLM Topic Extraction

The notebook configures an OpenAI-compatible client pointed at Google’s Gemini endpoint and uses the `gemini-2.5-flash-lite` model for extraction. A `Pydantic` schema named `DebateExtraction` is used to enforce a JSON output with a single field, `topic`. [file:2]

The prompt instructs the model to extract the main policy issue discussed in each speech and return a topic label that is:
- 1 to 5 words
- lowercase
- without punctuation [file:2]

The extraction loop processes all 1,000 speeches and stores the final structured output in:

- `extracted_debate_topicsyears.csv` [file:2]

### 3. Descriptive Exploration

Once topics are extracted, the notebook examines repeated topic labels and highlights the most frequent ones. Examples among the top repeated labels include `rule of law`, `gender violence`, `multiannual financial framework`, `energy prices and inflation`, and `digital services act`. [file:2]

The notebook also merges generated topics back with the original speech data and samples a few records for manual inspection. This helps assess whether the assigned topic is a reasonable summary of each translated speech. [file:2]

### 4. Network Analysis

The final section builds a bipartite graph linking speakers to the ten most common topics. Using `networkx` and bipartite betweenness centrality, the notebook identifies:
- central speakers, meaning speakers connected to many different topics
- central topics, meaning topics that bridge many different speakers [file:2]

## Requirements

The notebook imports the following main libraries:

- `os`
- `json`
- `pathlib`
- `time`
- `pandas`
- `numpy`
- `seaborn`
- `matplotlib`
- `tqdm`
- `networkx`
- `datasets`
- `openai`
- `python-dotenv`
- `pydantic` [file:2]

Install the core dependencies with:

```bash
pip install pandas numpy seaborn matplotlib tqdm networkx datasets openai python-dotenv pydantic
