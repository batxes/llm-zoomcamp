# Vector Search 

## 3.1 Introduction to Vector Search

<a href="https://www.youtube.com/watch?v=C5AWdL3kg1Q&list=PL3MmuxUbc_hIB4fSqLy_0AfTjVLpgjV3R">
  <img src="https://markdown-videos-api.jorgenkh.no/youtube/C5AWdL3kg1Q">
</a>

* [Slides](https://github.com/dataML007/elastic_search/blob/main/Introduction%20to%20Vector%20DB.pdf)


## 3.2 Semantic Search with Elasticsearch

<a href="https://www.youtube.com/watch?v=ptByfB_YcEg&list=PL3MmuxUbc_hIB4fSqLy_0AfTjVLpgjV3R">
  <img src="https://markdown-videos-api.jorgenkh.no/youtube/ptByfB_YcEg">
</a>

* Notebook: [demo_es.ipynb](demo_es.ipynb)

use docker:
```
docker run -it \
    --rm \
    --name elasticsearch \
    -p 9200:9200 \
    -p 9300:9300 \
    -e "discovery.type=single-node" \
    -e "xpack.security.enabled=false" \
    docker.elastic.co/elasticsearch/elasticsearch:8.4.3
```
if container is being used, do docker ps -a and remove the one with the same name

We will use the same environment as in week 1.
We will use the documents.json and create a documents file (collection of fields with their associated values) and using a pretrained model from huggingface, we will create the embeddings. With that, we will push into an index in Elasticssearch vector DB. When the are indexed in Elasticsearch, every time a user makes a search, it will create an embedding and then it will search in elasticsearch, giving later a semantic search result.

first pipenv install -r requirements.txt 

we also need: pip install sentence_transformers==2.7.0

pipenv install sentence_transformers
Now lets go through the notebook:

Sentence Transformer, a.k.a. as SBERT, will help you with a simple call, calling pretrained models in one line code. 
Then, thanks to this,  we can do the inference also in one line.

LINK in: https://www.sbert.net/docs/sentence_transformer/pretrained_models.html

every time yo pass a certain text to the model, you get back a vector.

Next we create the embeddings, 1 for each data in documents

next, connect to elasticsearch

now we need to create an index. For that we need first to create a mapping.
we need to provide the metadate to create a schema right? so here the same, the mapping will have all the metadata of the document data.

After that, we need to populate the index with the embeddings.
for everyt document pus it into the index, with a for loop

At this moment, all the black lines in the image of the notebook are done. Nowe we need to create a user query, step6.

Every term in the search term has to be converted into a vector as well. We do that with mode.encode.

and finally we seach with elasticsearch.



### 3.2.2 Advanced Semantic Search

<a href="https://www.youtube.com/watch?v=yb3nYGuIL4c&list=PL3MmuxUbc_hIB4fSqLy_0AfTjVLpgjV3R">
  <img src="https://markdown-videos-api.jorgenkh.no/youtube/yb3nYGuIL4c">
</a>


## 3.3 Evaluating Retrieval 

### 3.3.1 Introduction

<a href="https://www.youtube.com/watch?v=APMrUnC_dy0&list=PL3MmuxUbc_hIB4fSqLy_0AfTjVLpgjV3R">
  <img src="https://markdown-videos-api.jorgenkh.no/youtube/APMrUnC_dy0">
</a>

Plan for the section:

* Why do we need evaluation
* [Evaluation metrics](eval/evaluation-metrics.md)
    When searching with elasticsearch, we can choose in query: different type, different boosting coofecient, which fields we include and more things. We need to evaluate all different changes we do. 
    Some evaluation metrics are: precision at k (P@k), Recall, Mean Average Precisoon ... 
* Ground truth / gold standard data
* Generating ground truth with LLM
* Evaluating the search resuls


### 3.3.2 Getting ground truth data. Ground Truth Dataset Generation for Retrieval Evaluation

<a href="https://www.youtube.com/watch?v=bpxi6fKcyLw&list=PL3MmuxUbc_hIB4fSqLy_0AfTjVLpgjV3R">
  <img src="https://markdown-videos-api.jorgenkh.no/youtube/bpxi6fKcyLw">
</a>

* Approaches for getting evaluation data
* Using OpenAI to generate evaluation data

Links:

* [notebook](eval/ground-truth-data.ipynb)
* [documents with ids](eval/documents-with-ids.json)
* [queries generated by OpenAI (pickle)](eval/results.bin)
* [ground truth dataset](eval/ground-truth-data.csv)

For each record in FAQ we will generate 5 questions.
if we have 1000 records, we will have 5000 queries.o

We need an ID for each record/document.
we can give a random number to each document but that is not ideal...
Second best thing is to generate an ID based on the content. This is better because we do not depend on order, only in content. But if somebody changes the data then the ID would be different. Still this should be fine right now.

later we do the prompt and 

### 3.3.3 Ranking evaluation: text search

<a href="https://www.youtube.com/watch?v=fdIV4xCsp0c&list=PL3MmuxUbc_hIB4fSqLy_0AfTjVLpgjV3R">
  <img src="https://markdown-videos-api.jorgenkh.no/youtube/fdIV4xCsp0c">
</a>

* Elasticsearch with text results
* minsearch

Links:

* [Notebook](eval/evaluate-text.ipynb)

Here we will use the data created in previous video to validate text search results.
We will take the grounde truth data that we generated previously.

For each query in our ground_truth_datasets:
    we will execute the query
    check if document is in the results
    and we will use Hit rate and tge Mean reciproval Rank (MRR)

we will use evaluate_text.ipynb

I will run the elasticsearch server with docker first and index the documents.




### 3.3.4 Ranking evaluation: vector search

<a href="https://www.youtube.com/watch?v=VRprIm9-VV8&list=PL3MmuxUbc_hIB4fSqLy_0AfTjVLpgjV3R">
  <img src="https://markdown-videos-api.jorgenkh.no/youtube/VRprIm9-VV8">
</a>

* Elasticsearch with vector search
* Ranking with question, answer, question+answer embeddings

Links:

* [Notebook](eval/evaluate-vector.ipynb)

We will do the same as before but with vector search.



## Homework

See [here](../cohorts/2024/03-vector-search/homework.md)


# Notes

* Did you take notes? Add them above this line (Send a PR with *links* to your notes)
