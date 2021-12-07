## Table of contents
* [General info](#general-info)
* [Technologies](#technologies)
* [Setup](#setup)
* [Usage](#usage)

## General info
This project is used as an automated filtering tool to identify good leads for Toucan's marketing team. I used the [Sentence Transformers model](https://www.sbert.net/) and cosine similarity to find high quality matches between the customer data in Toucan's CRM database and profiles that Toucan's marketing team is looking to target. Confidential information has been anonymised using Python's Faker package.

	
## Technologies
Project is created with:
* Python version: 3.7.12

## Setup
To run this project, install package locally using Google Colab or Jupiter Notebook:

```
pip install -U sentence-transformers
```
## Usage
I created three functions for text preprocessing and one function to extract the best match for the CRM data. All codes are reusable by replacing the dataframe and column name.

```python
#function to remove stopwords in job title
def remove_stopwords (df_name, column_name):
    clean = [space.join([word for word in name.split() if word not in stopwords_dict]) \
             for name in df_name[column_name]]
    return clean
```

```python
#function to remove special signs in job title
def remove_sign(df_name, column_name):
  clean = [re.sub('[|!@#$-.&/_+={}()]', ' ', text) for text in df_name[column_name]]
  return clean
```

```python
#load pre-trained sentence transformer model
from sentence_transformers import SentenceTransformer
model = SentenceTransformer('distiluse-base-multilingual-cased-v1') #used the multilingual model - can replace with other models

#function to vectorize job title 
def text_vectorizer (df_name, column_name):
  vector = [model.encode(title) for title in df_name[column_name]]
  
  return vector
```

```python
#function to find a match for each job title in CRM data using the max value of cosine similarity: 

def get_matches(df_name_1, df_name_2, column_name_1, column_name_2):
  similarity = {}
  i = 0
  for vec1 in df_name_1[column_name_1]:
    similarity_persona = [cosine_similarity(vec1, vec2) for vec2 in df_name_2[column_name_2]]
    
    max_similiarity = max(similarity_persona)
    persona_id = similarity_persona.index(max_similiarity) 
    similarity[i] = [max_similiarity, persona_id]
    i += 1
  
  return similarity
```
