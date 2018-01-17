# Semantic Search using NLP and Wikipedia's API

## The Task

The objective of this project is to engineer a novel wikipedia search engine using data collection, infrastructure, and natural language processing.

The project has two components:
- Data collection
- Search algorithm development


### Notebook 1 -- Data_Collection

**Collected all of the articles** under the following wikipedia categories:

* [Machine Learning](https://en.wikipedia.org/wiki/Category:Machine_learning)
* [Business Software](https://en.wikipedia.org/wiki/Category:Business_software)

**Note:** Both "Machine Learning" and "Business Software" contain a heirarchy of nested sub-categories. Needed to build a workflow to discern between sub-categories and pages, and then execute different functions based on what is found at each subsequent level below the target categories. The recursion of this is explained in the self-defined functions below.

This collection was done using a series of defined functions:
`format_name(search_name)`: formats the text of the category, or `search_name`, using Regex so that it can be used in a request query
`get_cat_stuff(category)`: queries all sub-categories and pages underneath a specified Wikipedia `category`
`get_page_stuff(pageid)`: queries the text from a specified page, selecting said page by `pageid`
`cat_query_to_df(category)`: converts the output from `get_cat_stuff(category)` to a DataFrame
`page_query_to_dict(pageid)`: grabs just the text of the page from the `get_page_stuff(pageid)` output
`cat_scan(category, max_depth, p_category = None)`: function that recursively looks at everything underneath each category and sub-category. If it finds a page, it sends the output to the `page_scan` function below. If it's a sub-category, the function starts back at the beginning of the same function.
`page_scan(pageid, title, p_immed, p_category)`: from the prior `cat_scan()` function, takes the page output and saves each page's text in a dictionary with pertinent information (title, parent category, immediate parent sub-category, page id).

The raw page text and its category information was then written to a collection on a Mongo server running on a dedicated AWS instance.


### Notebook 2 -- Data_Cleaning

Defined a function `cleaner(text)` that uses regex to convert the text from Wikipedia pages collected in the prior step so that they are legible. 


### Notebook 3 -- Semantic_Search

Using Latent Semantic Analysis and cosine similarity specifically, I developed a query to find the top 5 related articles to a specified search term. Recall that it is only searching the pages/articles underneath the categories Machine Learning and Business Software on Wikipedia.

In the project, I ended up searching on the term 'mysql', finding the 5 most related articles to that search term and the associated cosine similarity between the term and the articles.


