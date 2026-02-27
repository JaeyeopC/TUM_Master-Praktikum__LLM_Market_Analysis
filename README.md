# Leveraging LLM Pipelines to Generate Insurance Product Comparison Data

## Project Summary

This repository implements a proof-of-concept, LLM-driven competitive market analysis system that generates structured insurance product comparison data across multiple companies in the German insurance market.

The project was developed as part of a **Master-Praktikum at the Technical University of Munich** and conducted within the **Social Computing Research Group at the TUM School of Computation, Information and Technology**.

At a high level, the system operationalizes a **“crawl → clean → extract → classify → compare → evaluate”** workflow through a reproducible pipeline architecture, aiming to improve **accuracy, consistency, and coverage of LLM-generated product comparison outputs** via prompt engineering and automated evaluation.

For more details about this project, please refer to the project report and poster included in this repository.

---

# What the Project Builds

The repository describes an **end-to-end pipeline** designed to produce product comparisons from publicly available insurer webpages.

It begins with **data collection by crawling insurer sites** to gather product pages, followed by filtering and transformation steps to remove irrelevant content and convert or normalize content for downstream processing.

Next, the pipeline performs **LLM-based extraction and classification of product details**.  
The project explicitly states that **GPT-4o** is the primary model used for classification and comparison generation, and that multiple prompting strategies. Finally, the system generates **product comparisons** by identifying similar products across providers using **embeddings and cosine similarity**.

---

## Repository Structure and Deliverables 
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

---

# Data Flow

The repository’s `data/` directory reflects a **multi-stage processing pipeline**, where each stage produces artifacts used by subsequent steps.

The stages include:

- **Crawled data** – raw data collected from insurance company webpages  
- **Filtered product pages** – webpages filtered to retain only relevant product information  
- **Filtered product markdowns** – webpages converted into normalized Markdown format  
- **Extracted product details** – structured product information extracted from text and stored in JSON format  
- **Classified product details** – product categories added to the structured data using LLM classification  
- **Product comparison markdowns** – generated comparison tables for similar insurance products  
- **Evaluation artifacts** – evaluation metrics and outputs, including the prompts used during the evaluation process

This staged design follows **reproducible data engineering practices**, enabling iterative improvement of upstream steps and clear observation of downstream impacts on comparison and evaluation results under different prompting strategies.

---

# Methods 

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

# Key Findings

The repository reports three main findings:

1. **Advanced classification prompts increase comprehensiveness but may reduce specificity**, indicating a trade-off that affects downstream comparison usefulness.

2. **LLM-as-a-judge metrics (via TruLens)** can measure factual alignment and completeness, providing a more systematic alternative to purely manual inspection.

3. **Combining structured prompting with metric-based evaluation improves reliability** of generated product comparisons.

---

## Limitations

Despite promising results, several limitations remain in the current pipeline.

First, the evaluation framework relies primarily on **LLM-as-a-judge metrics (via TruLens)**.  
While this approach enables scalable evaluation, previous studies highlight that LLM judges may introduce biases, such as favoring more verbose outputs or being sensitive to prompt design. As a result, evaluation scores may not always perfectly align with expert human judgment.

Second, the performance of the system is **highly sensitive to prompt design**.  
Small variations in prompt structure, instructions, or output formatting can significantly influence extraction accuracy and classification consistency. This makes prompt optimization an iterative and sometimes unstable process.

Third, the system currently focuses on a **limited set of insurance providers and product pages**.  
Although the pipeline is designed to be scalable, the present dataset may not fully capture the diversity of product structures and terminology across the broader insurance market.

---

## Future Work

Future work will focus on improving both **prompt optimization strategies and evaluation robustness**.

One direction is the exploration of more systematic prompt optimization techniques, such as automated prompt search, reflection-based prompt refinement, or chain-of-thought style prompting.

Another important extension is the development of **hybrid evaluation frameworks** that combine LLM-based metrics with human expert annotations to improve reliability and reduce potential evaluation bias.

Additionally, the pipeline could be expanded to include **a broader range of insurance providers and product categories**, enabling more comprehensive competitive market analysis.

Finally, further research could explore **alternative evaluation metrics and retrieval strategies**, including embedding-based validation methods or multi-model evaluation approaches to increase robustness.

---
## Configuration & Setup

### Base Configuration

The `conf/base/` folder contains **shared configuration files** used across the project.

These configurations include project-level settings such as dataset definitions, pipeline parameters, and other non-sensitive settings that can be safely shared among team members.

For more information about Kedro configuration, see the official documentation:  
https://docs.kedro.org/en/stable/configuration/configuration_basics.html

### Environment Setup

The project uses **Poetry** for dependency management and virtual environment handling.

Install dependencies:

```bash
poetry install
```

Activate the virtual environment:

```bash
poetry shell
```

### Running the Kedro Pipeline

Run the entire pipeline:

```bash
poetry run kedro run
```

This executes the pipeline in the order specified in:

`pipeline_registry.py`

Run a specific node:

```bash
poetry run kedro run --node <node_name>
```

Run a specific pipeline:

```bash
poetry run kedro run --pipeline <pipeline_name>
```

Run a specific node inside a specific pipeline:

```bash
poetry run kedro run --pipeline <pipeline_name> --nodes <node_name>
```


### Pipeline and Dataset Configuration

Pipeline definitions are located in: `pipeline_registry.py`

Dataset configurations are defined in: `conf/base/catalog.yml`

Supported dataset types can be found in the Kedro datasets documentation:
https://docs.kedro.org/projects/kedro-datasets/en/kedro-datasets-6.0.0/api/kedro_datasets.html


---


## Acknowledgments

**Team Members**

- Khalil Chikhaoui  
- Jaeyeop Chung  
- Umut Ekin Gezer  

**Advisor**

- Dr. Gerhard Johann Hagerer
