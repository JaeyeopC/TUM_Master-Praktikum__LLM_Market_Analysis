# Leveraging LLM Pipelines to Generate Insurance Product Comparison Data

## Project Summary

This repository implements a proof-of-concept, LLM-driven competitive market analysis system that generates structured insurance product comparison data across multiple companies in the German insurance market.

The project was developed as part of a **Master-Praktikum at the Technical University of Munich** and conducted within the **Social Computing Research Group at the TUM School of Computation, Information and Technology**.

At a high level, the system operationalizes a **“crawl → clean → extract → classify → compare → evaluate”** workflow through a reproducible pipeline architecture, aiming to improve **accuracy, consistency, and coverage of LLM-generated product comparison outputs** via prompt engineering and automated evaluation.

---

# What the Project Builds

The repository describes an **end-to-end pipeline** designed to produce product comparisons from publicly available insurer webpages.

It begins with **data collection by crawling insurer sites** to gather product pages, followed by filtering and transformation steps to remove irrelevant content and convert or normalize content for downstream processing.

Next, the pipeline performs **LLM-based extraction and classification of product details**.  
The project explicitly states that **GPT-4o** is the primary model used for classification and comparison generation, and that multiple prompting strategies are applied, including:

- Role-based prompting
- Iterative prompting
- Structured output prompting
- Constraint-based prompting

Finally, the system generates **product comparisons** by identifying similar products across providers using **embeddings and cosine similarity**.

Cosine similarity is commonly defined as a **normalized dot product between vectors**, yielding a bounded similarity score used to compare embeddings.

---

## Repository Structure and Deliverables 
The repository root contains a small set of top-level items, including a dedicated implementation directory and two PDF deliverables (**a report and a poster**).

The main implementation is organized under a **Kedro project structure**, with explicit references to configuration, datasets, notebooks, tests, and modular pipeline components.

The repository layout includes the following major elements:
```
NLP Competitive Market Analysis/
│── .viz/
│── conf/
│── data/
│   ├── 1_insurances_crawled_data/
│   ├── 2_filtered_product_pages/
│   ├── 3_filtered_product_markdowns/
│   ├── 4_extracted_product_details/
│   ├── 5_classified_product_details/
│   ├── 6_product_comparisons_markdowns/
│   ├── 8_evaluation/
│── lab_competitive_analysis/
│   ├── pipelines/
│   │   ├── insurance_data_classification/
│   │   ├── insurance_data_comparison/
│   │   ├── insurance_data_crawling/
│   │   ├── insurance_data_evaluation/
│   │   ├── insurance_data_extraction/
│   │   ├── insurance_data_filtering/
│   │   ├── insurance_data_markdown/
│   │   ├── reference_data/
│── notebooks/
│── tests/
│── pyproject.toml
│── README.md
│── poetry.lock

```

Two PDF files are present at the repository root and appear intended as formal deliverables for dissemination:

- Final project report  
- Project poster  

The project is released under the **MIT License**.

---

# Data Flow

The repository’s `data/` directory reflects a **multi-stage processing pipeline**, where each stage produces artifacts used by subsequent steps.

The stages include:

- **Crawled data** – raw insurer webpages  
- **Filtered product pages** – irrelevant content removed  
- **Filtered product markdowns** – normalized text representations  
- **Extracted product details** – structured fields derived from text  
- **Classified product details** – product categories and attributes assigned via LLM classification  
- **Product comparison markdowns** – comparison outputs formatted for review and analysis  
- **Evaluation artifacts** – metrics and evaluation outputs  

This staged design follows **reproducible data engineering practices**, allowing iterative improvement of upstream steps (such as filtering or extraction) and direct observation of downstream impacts on comparisons and evaluations.

---

# Methods and Technologies

The project is orchestrated using **Kedro**, a framework designed to build robust, modular, and maintainable data pipelines with consistent project structure and pipeline assembly.

For pipeline visibility and debugging, the project references **Kedro-Viz**, an interactive visualization tool for Kedro pipelines that supports exploration of pipeline graphs and metadata.

## Evaluation

For evaluation, the project uses **TruLens metrics**, specifically:

- **Groundedness**
- **Comprehensiveness**
- **GroundTruthAgreement**

TruLens documentation describes these metric families as:

- **Groundedness** – traceability of generated outputs to the original source information  
- **Comprehensiveness** – coverage of relevant key points in the generated output  
- **Ground-truth agreement** – similarity between generated output and reference answers  

Conceptually, this evaluation strategy aligns with the broader pattern known as **LLM-as-a-judge**, where LLM-based systems are used to evaluate model outputs.

Research indicates that while LLM-as-judge approaches can be scalable and useful, they require careful design to ensure reliability and alignment with human intent, and should be considered **decision-support tools rather than definitive ground truth**.

## Similarity Matching

To identify comparable products across insurance providers, the project uses **embeddings combined with cosine similarity**.

Cosine similarity is commonly implemented as a **normalized dot product between embedding vectors**, enabling semantic similarity comparisons for retrieval and matching tasks.

---

# Key Findings and Limitations

The repository reports three main findings:

1. **Advanced classification prompts increase comprehensiveness but may reduce specificity**, indicating a trade-off that affects downstream comparison usefulness.

2. **LLM-as-a-judge metrics (via TruLens)** can measure factual alignment and completeness, providing a more systematic alternative to purely manual inspection.

3. **Combining structured prompting with metric-based evaluation improves reliability** of generated product comparisons.

Planned improvements include:

- Introducing more advanced **chain-of-thought prompting strategies**
- Expanding **insurance provider coverage**
- Exploring **additional evaluation metrics** for optimization

---

# Limitations of This Overview

A practical limitation in this analysis is that, although the repository contains two PDF deliverables (report and poster), the browsing environment was unable to directly retrieve and parse the PDF contents from the repository’s file endpoints.

Requests consistently failed with fetch/cache errors.

As a result, the overview above is grounded primarily in:

- the repository’s **README description**
- documentation for the cited tools
- publicly available methodology references

rather than the full content of the PDF deliverables.


## Acknowledgments
This work was conducted as part of the **Social Computing Research Group** at the **TUM School of Computation, Information and Technology**, Technical University of Munich. 

**Team Members**

- Khalil Chikhaoui  
- Jaeyeop Chung  
- Umut Ekin Gezer  

**Advisor**

- Dr. Gerhard Johann Hagerer
