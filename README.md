# SAGE: SUQL-based Agentic Guided Event-Detection
A conversational SUQL-powered agent that navigates long-form video, retrieves key events, and explains them through natural, interactive dialogue.

### Methods:
* Pre-processing videos to collect metadata: `preprocess_videos.ipynb`
* Agentic: `agentic.ipynb`
* Hybrid SUQL: `<TODO ZAYN>`


#### Pre-processing Videos
This code is to process the videos into 5 minute chunks that collect meta data for each of those chunks and save them into a csv file. The metadata contains a mix of audio and visual information that gives the reader a holistic view of the events in chunks as well as the relevant contextual information surrounding them.

To setup:
* You need your own GCP project which you will then use to call the gemini API via google's vertexai. Using gemini's API from google AI studio doesn't give us similar results.
* If you can't setup your own GCP project, use the output csv file and video we've provided to test out the two methods.

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

#### Hybrid SUQL Method
`<TODO ZAYN>`

