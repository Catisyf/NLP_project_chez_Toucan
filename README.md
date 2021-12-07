## Table of contents
* [General info](#general-info)
* [Technologies](#technologies)
* [Setup](#setup)
* [Usage](#usage)

## General info
This project is used as an automated filtering tool to identify good leads for Toucan's marketing team. I used the [Sentence Transformers model](https://www.sbert.net/) and cosine similarity to find high quality matches between the customer data in Toucan's CRM database and the profiles Toucan's marketing team is looking to target. Confidential information has been anonymised using Python's Faker package.

	
## Technologies
Project is created with:
* Python version: 3.6

## Setup
To run this project, install packages locally using Google Colab or Jupiter Notebook:

```
pip install -U sentence-transformers
```
## Usage
I created three functions for text preprocessing. The code is reusable by replacing the dataframe name and column name.

```python
#function to remove stopwords
def remove_stopwords (df_name, column_name):
    clean = [space.join([word for word in name.split() if word not in stopwords_dict]) \
             for name in df_name[column_name]]
    return clean
```

```python
#function to remove special signs
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
