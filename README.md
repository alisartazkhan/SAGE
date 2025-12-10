# SAGE: SUQL-based Agentic Guided Event-Detection
A conversational SUQL-powered agent that navigates long-form video, retrieves key events, and explains them through natural, interactive dialogue.

### Methods:
* Pre-processing videos to collect metadata: `pre_process_videos.ipynb`
* Agentic: `agentic.ipynb`
* Hybrid SUQL: `SUQL_CS224V.ipynb`


#### Pre-processing Videos
This code is to process the videos into 5 minute chunks that collect meta data for each of those chunks and save them into a csv file. The metadata contains a mix of audio and visual information that gives the reader a holistic view of the events in chunks as well as the relevant contextual information surrounding them.

To setup:
* You need your own GCP project which you will then use to call the gemini API via google's vertexai. Using gemini's API from google AI studio doesn't give us similar results.
* If you can't setup your own GCP project, use the sample csv files and videos we've provided to test out the two methods: [Gdrive Link](https://drive.google.com/drive/folders/1zDFP7yLmGo_1uUIfTsRKhwX-WD8WPacU?usp=sharing).

#### Agentic Method
This notebook implements our agentic video QA pipeline over long-form body-cam metadata. Given a user question and the full (or candidate) metadata table, an orchestrator LLM iteratively plans tool calls to:
 - select the most relevant time segments from metadata,
 - view/visualize those segments for evidence
 - ask a multimodal LLM (Gemini) targeted follow-up questions when the required detail is not explicitly present in the metadata.

 The agent loops until it gathers sufficient evidence, then returns a verified answer grounded in timestamps + retrieved rows + optional visual confirmation.

Setup the following API Keys in google colab secrets:
 - LLM_API_ENDPOINT
 - LLM_API_DEPLOYMENT
 - LLM_API_KEY
 - LLM_API_VERSION
 - GEMINI_API_KEY
<img width="531" height="294" alt="image" src="https://github.com/user-attachments/assets/ff306a51-65c6-44db-b42d-d990e9621ff6" />

### Hybrid SUQL Method (`SUQL_CS224V.ipynb`)
This notebook implements the structured retrieval layer using **SUQL (Semantic SQL)**. The primary goal is to create a **hallucination-free** agent that provides auditable answers. When a question cannot be answered from the database schema, it will explicitly respond with "I DON'T KNOW" rather than guessing.

**How it works:**
The notebook establishes an in-memory PostgreSQL database and launches two local services:
1.  **SUQL Embedding Server:** Embeds textual columns (descriptions, summaries) into a vector space for semantic search.
2.  **Free-Text Functions Server:** Provides the `answer()` predicate, allowing the LLM to perform semantic classification on text fields within a standard SQL query.

**Setup & Usage:**
1.  **API Keys:** Ensure your `OPENAI_API_KEY` and other required secrets are configured in the environment.
2.  **Required Files:** You **must** upload the following files to your Colab environment before running:
    * `bodycam.prompt` (The schema-grounded system prompt).
    * Your metadata CSV file (e.g., `17-122-1019 BWC_Redacted-1.csv`).
3.  **Sequential Execution:** Run all cells sequentially from top to bottom. This ensures the background embedding and free-text servers are initialized before the database is queried.

