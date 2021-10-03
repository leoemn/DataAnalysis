#<center>Machine Learning Modeling</center>

## Loading Data

### Loading flat files

In flat files, data is stored as plain text where each individual line refers to one row of data. The values corresponding to different fields are separated by a delimiter, usually `,` for CSVs but sometimes also `;` since it causes less problems with decimal points occurs less frequent in text. 

Consider the following example data of Hungarian chickenpox cases which can be downloaded from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Hungarian+Chickenpox+Cases) and is stored in the `hungary_chickenpox.csv` file:
 ```diff
   Date,BUDAPEST,BARANYA,BACS,BEKES,BORSOD,CSONGRAD,FEJER,GYOR,HAJDU,HEVES,JASZ,KOMAROM,NOGRAD,PEST,SOMOGY,SZABOLCS,TOLNA,VAS,VESZPREM,ZALA
   03/01/2005,168,79,30,173,169,42,136,120,162,36,130,57,2,178,66,64,11,29,87,68
   10/01/2005,157,60,30,92,200,53,51,70,84,28,80,50,29,141,48,29,58,53,68,26
   17/01/2005,96,44,31,86,93,30,93,84,191,51,64,46,4,157,33,33,24,18,62,44
 ```

 Flat files are easily loaded as `pandas.DataFrames` objects using the `read_csv` function:

  ```python
  import pandas as pd
  
  df = pd.read_csv('FILE_PATH/hungary_chickenpox.csv')
  df.head()
  ```

  ```diff
            Date	    BUDAPEST	BARANYA	BACS	BEKES	BORSOD	CSONGRAD	FEJER	GYOR	HAJDU	...	JASZ	KOMAROM	NOGRAD	PEST	SOMOGY	SZABOLCS	TOLNA	VAS	VESZPREM	ZALA
    0	    03/01/2005	168	        79	    30	    173	    169	        42	    136	    120	    162	    ...	130	    57	    2	    178	    66	    64	        11	    29	87	        68
    1	    10/01/2005	157	        60	    30	    92	    200	        53	    51	    70	    84	    ...	80	    50	    29	    141	    48	    29	        58	    53	68	        26
    2	    17/01/2005	96	        44	    31	    86	    93	        30	    93	    84	    191	    ...	64	    46	    4	    157	    33	    33	        24	    18	62	        44
    3	    24/01/2005	163	        49	    43	    126	    46	        39	    52	    114	    107	    ...	63	    54	    14	    107	    66	    50	        25	    21	43	        31
    4	    31/01/2005	122	        78	    53	    87	    103	        34	    95	    131	    172	    ...	61	    49	    11	    124	    63	    56	        7	    47	85	        60
    5 rows × 21 columns
  ```

The default delimiter for the `read_csv` function is `,` but one may pass the correct delimiter for the data at hand explicitly:
```python
df = pd.read_csv('FILE_PATH/hungary_chickenpox.csv', sep = ',')
```
#### Limiting columns 
* Choose columns to load with the `usecols` keyword argument.
* Accepts a list of columns number or names, or a function to filter column names.

```python
 import pandas as pd
 
 col_names = ['Date', 'BUDAPEST', 'BARANYA', 'BACS']
 col_nums = [0,4,5,6] 
 # Choose columns to load by name
 df_names = pd.read_csv('File_Path/hungary_chickenpox.csv',usecols=col_names)
 # Choose columns to load by number
 df_nums = pd.read_csv('File_Path/hungary_chickenpox.csv',usecols=col_nums)
```
### 2.Importing data from tmdbAPI
* Request an API key from TMDB
	1. create a free account on [themoviedb.org](https://www.themoviedb.org/signup)
	2. Visit the API Settings page in your Account Settings and request an API key
	3. It is very important to refer to API documentation to request the data we desire.
* Make API Call to TMDB to return the data
The most common library in python for making requests and working with APIs is the requests library.
we will first import this library.
```python
import requests
```
We’ll use the `requests.get()` function, which requires the URL we want to request as an argument. The `get()` function returns `response` object. 
```python
response = requests.get('https://api.themoviedb.org/3/discover/movie?api_key=' +  api_key + '&primary_release_year=2020&sort_by=revenue.desc')
```
if your request is succesfull you will receive response code 200, we can check it as below.
```python
print(response)
response.status_code
```
```diff
[Output]:
 <Response [200]>
 200
```
use the response.json() method to store parsed json response
```python
highest_revenue = response.json()
print(highest_revenue)
```
```diff
{'page': 1, 'results': [{'adult': False, 'backdrop_path': '/owraiceOKtSOa3t8sp3wA9K2Ox6.jpg', 'genre_ids': [16, 28, 12, 878], 'id': 703771, 'original_language': 'en',
'original_title': 'Deathstroke: Knights & Dragons - The Movie', 'overview': 'The assassin Deathstroke tries to save his family from the wrath of H.I.V.E. and the murderous
Jackal.', 'popularity': 4794.605, 'poster_path': '/vFIHbiy55smzi50RmF8LQjmpGcx.jpg', 'release_date': '2020-08-04', 'title': 'Deathstroke: Knights & Dragons - The Movie',
'video': False, 'vote_average': 7, 'vote_count': 247}
```
JSON output looks like python dictionary and list. we can use `results` key to save it as python list.
```python
highest_revenue_films = highest_revenue['results']
```
now we can create a dataframe and store this list as dataframe
```python
# define column names for our new dataframe
columns = ['adult', 'original_language', 'original_title', 'release_date']
# create dataframe with adult, original_language, original_title, release_date columns
df_api = pd.DataFrame(highest_revenue_films, columns = columns)
```
