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
using beautiful soup ``` pip install BeautifulSoup```
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



## MapReduce jobs

## MRUnit test

