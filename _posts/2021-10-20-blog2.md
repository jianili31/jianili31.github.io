---
layout: post
title: My Blog 2
---

# Make a Web Scraper for Movie/TV Recommendation

In this blog, I will demonstrate how to make a web scraper that extracts information from a movie/TV show's IMDB page, from which we can determine movies/TV shows that share the most actors with our favorite movie/TV shows. 

Here is the link to my GitHub repository: https://github.com/jianili31/imdb_scraper

## Step-by-Step Description of the IMDB Web Scraper

This web scraper assumes that we start from the IMDB page of our favorite movie/TV show. In this demo, I am going to use Black Mirror as an example. 

The first method, parse(), finds and navigates to the Cast&Crew section of the web page, where we see the full list of actors in Black Mirror.

```python
def parse(self, response):
    """
    This method finds and navigates to the Cast&Crew section of the web page, 
    and calls parse_full_credits()
    """
    # Cast&Crew button is nexted in a list of class "ipc-inline-list__item"
    # get the url associated with the button
    cast = response.css("li.ipc-inline-list__item a")[2].attrib["href"]
    # construct the absolute path
    cast = response.urljoin(cast)
    # call parse_full_credits()
    yield scrapy.Request(cast, callback = self.parse_full_credits)
```

The second method, parse_full_credits(), finds and navigates to each actor's web page, where we can find information about each actor's filmography.

```python
def parse_full_credits(self, response):
    """
    This method finds and navigates to each actor's web page, 
    and calls parse_actor_page()
    """ 
    # for each actor, find their photo  
    for actor in response.css("td.primary_photo a"):
      # get the url associated with the photo
      actor_page = actor.attrib["href"]
      # construct the absolute path
      actor_page = response.urljoin(actor_page)
      # call parse_actor_page()
      yield scrapy.Request(actor_page, callback = self.parse_actor_page)
```

Finally, the parse_actor_page() method extracts the actor's name and movies/TV shows they have played, and return a dictionary storing the relevant information. 

```python
  def parse_actor_page(self, response):
    """
    This method extracts the actor's name and movies/TV shows they have played, 
    and return a dictionary storing the relevant information
    """ 
    # extract actor's name    
    actor_name = response.css("h1.header").css("span.itemprop::text").get()
    # for each work under the Filmograph section, get the name
    for work in response.css("div.filmo-row b"):
      # get the name of the movie/TV show
      movie_or_TV_name = work.css("a::text").get()
      yield { # a generator that spits out a dictionary of actor's name and the movie/TV show one at a time
        "actor_name": actor_name,
        "movie_or_TV_name": movie_or_TV_name
      }

```

## Implementation

To run the scraper, follow these steps:
1. Download the imdb_scraper repository from my GitHub
2. Open Terminal on your laptop, cd into the folder that contains the downloaded GitHub repository
3. Type *scrapy crawl imdb_spider -o [FILENAME]* in the command line to run the scraper and save information into a file called FILENAME

## Results

Finally, here is a visualization of the shared actors in Black Mirror.

```python
import pandas as pd
```


```python
# read in results.csv
results = pd.read_csv("results.csv")
```


```python
# count the number of times each movie/TV appears in the dataframe, 
# which is equal to the number of actors in Black Mirror who are also in the movie/TV
shared_actors = pd.DataFrame(results['movie_or_TV_name'].value_counts())
# reset index so that movie/TV names can be treated as a normal column
shared_actors = shared_actors.reset_index()
# rename the columns
shared_actors.columns = ['Movie/TV', 'Number of Shared Actors']
```


```python
# display the first 20 movies/TVs with the most shared actors
shared_actors.head(20)
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
      <th>Movie/TV</th>
      <th>Number of Shared Actors</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Black Mirror</td>
      <td>506</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Doctors</td>
      <td>93</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Holby City</td>
      <td>78</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Casualty</td>
      <td>76</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The Bill</td>
      <td>70</td>
    </tr>
    <tr>
      <th>5</th>
      <td>EastEnders</td>
      <td>62</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Midsomer Murders</td>
      <td>59</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Silent Witness</td>
      <td>55</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Entertainment Tonight</td>
      <td>54</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Doctor Who</td>
      <td>50</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Celebrity Page</td>
      <td>47</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Made in Hollywood</td>
      <td>47</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Good Morning America</td>
      <td>38</td>
    </tr>
    <tr>
      <th>13</th>
      <td>The Late Late Show with James Corden</td>
      <td>37</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Death in Paradise</td>
      <td>36</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Lorraine</td>
      <td>33</td>
    </tr>
    <tr>
      <th>16</th>
      <td>New Tricks</td>
      <td>32</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Breakfast</td>
      <td>30</td>
    </tr>
    <tr>
      <th>18</th>
      <td>MI-5</td>
      <td>30</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Inspector Lewis</td>
      <td>28</td>
    </tr>
  </tbody>
</table>
</div>


