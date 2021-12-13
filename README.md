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
def remove_stopwords (df_column):
    clean = [space.join([word for word in name.split() if word not in stopwords_dict]) \
             for name in df_column]
    return clean
```

```python
#function to remove special signs in job title
def remove_sign(df_column):
  clean = [re.sub('[|!@#$-.&/_+={}()]', ' ', text) for text in df_column]
  return clean
```

```python
#function to vectorize job title
#use the broadcasting function in numpy array to speed up processing: reduced run time to half
def text_vectorizer (df_column):
  vector = model.encode(df_column.values)
  
  return vector.tolist()
```

```python
#function to find a match for each job title in CRM data using the max value of cosine similarity 

def get_matches(df_column_1, df_column_2):
  similarity = {}
  i = 0
  for vec1 in df_column_1:
    similarity_persona = [cosine_similarity(vec1, vec2) for vec2 in df_column_2]
    
    max_similiarity = max(similarity_persona)
    persona_id = similarity_persona.index(max_similiarity) 
    similarity[i] = [max_similiarity, persona_id]
    i += 1
  
  return similarity
```
