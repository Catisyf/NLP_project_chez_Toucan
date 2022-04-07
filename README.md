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
