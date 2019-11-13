
# Working with Known JSON Schemas - Lab

## Introduction
In this lab, you'll practice working with JSON files whose schema you know beforehand.

## Objectives
You will be able to:
* Use the JSON module to load and parse JSON documents
* Write data to predefined JSON schemas
* Convert JSON to a pandas dataframe

## Reading a JSON Schema

Here's the JSON schema provided for a section of the NY Times API:
<img src="images/nytimes_movie_schema.png" width=500>

or a fully expanded view:

<img src="images/nytimes_movie_schema_detailed.png" width=500>

You can more about the documentation [here](https://developer.nytimes.com/docs/movie-reviews-api/1/routes/reviews/%7Btype%7D.json/get).

You can see that the master structure is a dictionary and has a key named 'response'. This is also a dictionary and has two keys: 'data' and 'meta'. As you continue to examine the schema hierarchy, you'll notice the vast majority, in this case, are dictionaries. 

## Loading the Data File

Start by importing the json file. The sample response from the api is stored in a file **ny_times_movies.json**


```python
import json
f = open('ny_times_movies.json', 'r')
data = json.load(f)
```

## Loading Specific Data

Create a DataFrame of the major data container within the json file, listed under the 'results' heading in the schema above.


```python
import pandas as pd
df = pd.DataFrame(data['results'])
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>display_title</th>
      <th>mpaa_rating</th>
      <th>critics_pick</th>
      <th>byline</th>
      <th>headline</th>
      <th>summary_short</th>
      <th>publication_date</th>
      <th>opening_date</th>
      <th>date_updated</th>
      <th>link</th>
      <th>multimedia</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Can You Ever Forgive Me</td>
      <td>R</td>
      <td>1</td>
      <td>A.O. SCOTT</td>
      <td>Review: Melissa McCarthy Is Criminally Good in...</td>
      <td>Marielle Heller directs a true story of litera...</td>
      <td>2018-10-16</td>
      <td>2018-10-19</td>
      <td>2018-10-17 02:44:23</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Charm City</td>
      <td></td>
      <td>1</td>
      <td>BEN KENIGSBERG</td>
      <td>Review: ‘Charm City’ Vividly Captures the Stre...</td>
      <td>Marilyn Ness’s documentary is dedicated to the...</td>
      <td>2018-10-16</td>
      <td>2018-04-22</td>
      <td>2018-10-16 11:04:03</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Horn from the Heart: The Paul Butterfield Story</td>
      <td></td>
      <td>1</td>
      <td>GLENN KENNY</td>
      <td>Review: Paul Butterfield’s Story Is Told in ‘H...</td>
      <td>A documentary explores the life of the blues m...</td>
      <td>2018-10-16</td>
      <td>2018-10-19</td>
      <td>2018-10-16 11:04:04</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
    </tr>
    <tr>
      <td>3</td>
      <td>The Price of Everything</td>
      <td></td>
      <td>0</td>
      <td>A.O. SCOTT</td>
      <td>Review: ‘The Price of Everything’ Asks $56 Bil...</td>
      <td>This documentary examines the global art marke...</td>
      <td>2018-10-16</td>
      <td>2018-10-19</td>
      <td>2018-10-16 16:08:03</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Impulso</td>
      <td></td>
      <td>0</td>
      <td>BEN KENIGSBERG</td>
      <td>Review: ‘Impulso’ Goes Backstage With a Flamen...</td>
      <td>This documentary follows Rocío Molina, a cutti...</td>
      <td>2018-10-16</td>
      <td>None</td>
      <td>2018-10-16 11:04:03</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
    </tr>
  </tbody>
</table>
</div>



## How many unique critics are there?


```python
df.byline.nunique()
```




    7



## Create a new column for the review's url. Title the column 'review_url'


```python
df['review_url'] = df['link'].map(lambda x: x['url'])
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>display_title</th>
      <th>mpaa_rating</th>
      <th>critics_pick</th>
      <th>byline</th>
      <th>headline</th>
      <th>summary_short</th>
      <th>publication_date</th>
      <th>opening_date</th>
      <th>date_updated</th>
      <th>link</th>
      <th>multimedia</th>
      <th>review_url</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Can You Ever Forgive Me</td>
      <td>R</td>
      <td>1</td>
      <td>A.O. SCOTT</td>
      <td>Review: Melissa McCarthy Is Criminally Good in...</td>
      <td>Marielle Heller directs a true story of litera...</td>
      <td>2018-10-16</td>
      <td>2018-10-19</td>
      <td>2018-10-17 02:44:23</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
      <td>http://www.nytimes.com/2018/10/16/movies/can-y...</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Charm City</td>
      <td></td>
      <td>1</td>
      <td>BEN KENIGSBERG</td>
      <td>Review: ‘Charm City’ Vividly Captures the Stre...</td>
      <td>Marilyn Ness’s documentary is dedicated to the...</td>
      <td>2018-10-16</td>
      <td>2018-04-22</td>
      <td>2018-10-16 11:04:03</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
      <td>http://www.nytimes.com/2018/10/16/movies/charm...</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Horn from the Heart: The Paul Butterfield Story</td>
      <td></td>
      <td>1</td>
      <td>GLENN KENNY</td>
      <td>Review: Paul Butterfield’s Story Is Told in ‘H...</td>
      <td>A documentary explores the life of the blues m...</td>
      <td>2018-10-16</td>
      <td>2018-10-19</td>
      <td>2018-10-16 11:04:04</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
      <td>http://www.nytimes.com/2018/10/16/movies/horn-...</td>
    </tr>
    <tr>
      <td>3</td>
      <td>The Price of Everything</td>
      <td></td>
      <td>0</td>
      <td>A.O. SCOTT</td>
      <td>Review: ‘The Price of Everything’ Asks $56 Bil...</td>
      <td>This documentary examines the global art marke...</td>
      <td>2018-10-16</td>
      <td>2018-10-19</td>
      <td>2018-10-16 16:08:03</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
      <td>http://www.nytimes.com/2018/10/16/movies/the-p...</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Impulso</td>
      <td></td>
      <td>0</td>
      <td>BEN KENIGSBERG</td>
      <td>Review: ‘Impulso’ Goes Backstage With a Flamen...</td>
      <td>This documentary follows Rocío Molina, a cutti...</td>
      <td>2018-10-16</td>
      <td>None</td>
      <td>2018-10-16 11:04:03</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
      <td>http://www.nytimes.com/2018/10/16/movies/impul...</td>
    </tr>
  </tbody>
</table>
</div>



## How many results are in the file?


```python
data['num_results']
```




    20



## Summary
Well done! Here you continued to gather practice extracting data from JSON files and transforming them into our standard tool of Pandas DataFrames.
