# Information Retrieval Search Engine

A classical Information Retrieval (IR) system built with Python to search a snapshot of Ferdowsi University's English website.

The project demonstrates the main stages of a search engine: document collection, text preprocessing, inverted indexing, Boolean retrieval, TF-IDF weighting, and cosine-similarity ranking.

## Features

- **Document collection:** Loads a supplied collection of university web pages (title, URL, and extracted text).
- **Data cleaning:** Normalizes URLs and removes duplicate URL aliases and exact duplicate page content.
- **Text preprocessing:** Lowercasing, tokenization, a fixed English stopword list embedded in the notebook, short-token filtering, and Porter stemming. No NLTK corpus download is required.
- **Inverted index:** Maps each normalized term to the documents containing it and records document frequency.
- **Boolean retrieval:** Supports AND queries using posting-list intersection.
- **Ranked retrieval:** Builds normalized TF-IDF vectors and ranks documents using cosine similarity.
- **Performance inspection:** Runs example queries and reports median in-memory query latency.
- **Evaluation helpers:** Includes Precision@K, Recall@K, and F1@K functions for use with human relevance judgments.

## Repository structure

```text
IR_Search_Engine/
├── IR_Search_Engine.ipynb
├── README.md
└── data/
    └── crawled_data.json
```

## Getting started

1. Clone or download this repository.
2. Open `IR_Search_Engine.ipynb` in Jupyter Notebook, JupyterLab, or Google Colab.
3. Run the notebook cells from top to bottom.

The first setup cell installs the required packages. The notebook embeds its English stopword list, so it does not need to download the NLTK stopwords corpus:

```bash
pip install requests beautifulsoup4 lxml nltk pandas matplotlib
```

The notebook loads the included dataset from `data/crawled_data.json`. Keep the `data` folder beside the notebook when running locally.

## Dataset

The included JSON file is a snapshot of public pages from Ferdowsi University's English website. Each record contains:

- `url`: the source page URL
- `title`: the page title
- `content`: extracted page text

The supplied snapshot contains **522 source records**. The notebook normalizes HTTP/HTTPS URL variants and removes exact duplicate page bodies before indexing. On this snapshot, the resulting collection contains **222 unique documents**.

The notebook derives a readable title from the URL when the source page only provides the website-wide generic title.

## Retrieval pipeline

1. **Load documents** from the local JSON snapshot.
2. **Normalize and deduplicate** URLs and exact duplicate page bodies.
3. **Preprocess text** consistently for documents and queries.
4. **Build an inverted index** and document-frequency statistics.
5. **Run Boolean AND search** by intersecting posting lists.
6. **Build TF-IDF vectors** for documents and queries.
7. **Rank results** by cosine similarity.
8. **Inspect latency** over a small set of example queries.

The TF-IDF weighting used in the notebook is:

\[
TF(t,d)=\frac{f_{t,d}}{|d|}, \qquad IDF(t)=\log\left(\frac{N}{DF(t)}\right)
\]

\[
TFIDF(t,d)=TF(t,d)\times IDF(t)
\]

Document and query vectors are L2-normalized, so their dot product is cosine similarity.

## Example queries

The notebook includes example searches such as:

- `computer engineering`
- `international students`
- `research laboratories`
- `science faculty`
- `campus facilities libraries`

You can change the query in the ranked-search section to explore the collection.

## Optional crawler

A basic breadth-first crawler is included in the notebook, but it is **disabled by default**:

```python
RUN_CRAWLER = False
```

The crawler checks the website's `robots.txt`, restricts requests to `en.um.ac.ir`, and applies a delay between requests. The target website may disallow crawling. If it does, the crawler intentionally stops; use the included dataset instead. Do not bypass the website's crawling policy.

## Technologies

- Python
- Jupyter Notebook
- NLTK (Porter stemmer)
- BeautifulSoup
- Requests
- Pandas
- Matplotlib

