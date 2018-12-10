# Introduction

**Designed and developed a movie recommender system based on Netflix user rating history.**

1.  Utilized data manipulation in Jupiter notebook to integrate and clean the raw data.

2.  Applied total 4 MapReduce jobs from preprocessing raw data to getting the final rating results.

3.  Implemented Item CF(collaborative filtering) to calculate and normalize co-occurrence matrix.

4.  Deployed MRUnit to test the MapReduce job logic and monitor the output.

*the data stored in repository here is sample data* 


## Data preprocessing
(Jupyter Notebook environment)

### visit the website

Crawled raw data from Netflix
Python using urllib
```python
from urllib import request
resp = request.urlopen('https://netflix.com/')
html_data = resp.read().decode('utf-8')
```
### extract from html
using beautiful soup ``` pip install BeautifulSoup```
to get the user_id, user_comments, movie_id, movie, user_rating
```python
BeautifulSoup(html,"html.parser")

from bs4 import BeautifulSoup as bs
soup = bs(html_data, 'html.parser')    
nowplaying_movie = soup.find_all('div', id='nowplaying')
nowplaying_movie_list = nowplaying_movie[0].find_all('li', class_='list-item') 

nowplaying_list = [] 
for item in nowplaying_movie_list:        
        nowplaying_dict = {}        
        nowplaying_dict['id'] = item['data-subject']       
        for tag_img_item in item.find_all('img'):            
            nowplaying_dict['name'] = tag_img_item['alt']            
            nowplaying_list.append(nowplaying_dict)
            
requrl = 'https://netflix.com/subject/' + nowplaying_list[0]['id'] + '/comments' +'?' +'start=0' + '&limit=20' 
resp = request.urlopen(requrl) 
html_data = resp.read().decode('utf-8') 
soup = bs(html_data, 'html.parser') 
comment_div_lits = soup.find_all('div', class_='comment')

eachCommentList = []; 
for item in comment_div_lits: 
        if item.find_all('p')[0].string is not None:     
            eachCommentList.append(item.find_all('p')[0].string)
```
### data cleaning
filter the useless rating history with spam comments, no comments, intended poor reviews, etc

filter the meaningless datas and only remain movie_id paired with movie, user_rating paired with user_id

Mainly using Pandas and regex(re library), numpy, etc

```python
comments = ''
for k in range(len(eachCommentList)):
    comments = comments + (str(eachCommentList[k])).strip()

import re

pattern = re.compile(r'[\u4e00-\u9fa5]+')
filterdata = re.findall(pattern, comments)
cleaned_comments = ''.join(filterdata)

import jieba    
import pandas as pd  

segment = jieba.lcut(cleaned_comments)
words_df=pd.DataFrame({'segment':segment})

stopwords=pd.read_csv("stopwords.txt",index_col=False,quoting=3,sep="\t",names=['stopword'], encoding='utf-8')#quoting=3全不引用
words_df=words_df[~words_df.segment.isin(stopwords.stopword)]
```


## MapReduce jobs

### choose Item CF
>User CF 
 a form of collaborative filtering based on this similarity between users calculated using people’s ratings of those items
>item CF
 a form of collaborative filtering based on this similarity between items calculated using people’s ratings of those items

Construct the mapreduce job queue and inject dependencies using maven.

Each job queue follows the walkthrough: 
1. import data from HDFS which stores data in block files
2. read and pair with the raw data through the mapper and the reducer to process the ra data
3. calculate co-occurrence matrix and normalize the co-occurrence matrix
4. 
## MRUnit test

