# Introduction

**Designed and developed a movie recommender system based on Netflix user rating history.**

1.  Utilized data manipulation in Jupiter notebook to integrate and clean the raw data.

2.  Applied total 4 MapReduce jobs from preprocessing raw data to getting the final rating results.

3.  Implemented Item CF(collaborative filtering) to calculate and normalize co-occurrence matrix.

4.  Deployed MRUnit to test the MapReduce job logic and monitor the output.


## Data preprocessing

# visit the website

Crawled raw data from Netflix
Python using urllib
```python
from urllib import request
resp = request.urlopen('https://netflix.com/')
html_data = resp.read().decode('utf-8')
```
# extract from html
using beautiful soup ```python pip install BeautifulSoup```
```
BeautifulSoup(html,"html.parser")
```


## MapReduce jobs

## MRUnit test

