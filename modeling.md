#<center>Machine Learning Modeling</center>

## Loading Data

 ### 1.Importing CSV Data using Pandas
 
 * Sample of A spatio-temporal dataset of weekly chickenpox cases from Hungary `hungary_chickenpox.csv`
 ```diff
   Date,BUDAPEST,BARANYA,BACS,BEKES,BORSOD,CSONGRAD,FEJER,GYOR,HAJDU,HEVES,JASZ,KOMAROM,NOGRAD,PEST,SOMOGY,SZABOLCS,TOLNA,VAS,VESZPREM,ZALA
   03/01/2005,168,79,30,173,169,42,136,120,162,36,130,57,2,178,66,64,11,29,87,68
   10/01/2005,157,60,30,92,200,53,51,70,84,28,80,50,29,141,48,29,58,53,68,26
   17/01/2005,96,44,31,86,93,30,93,84,191,51,64,46,4,157,33,33,24,18,62,44
 ```
 ```python
  #importing pandas library 
  import pandas as pd
  
  #creating DataFrame df using pandas
  df = pd.read_csv('File_Path/hungary_chickenpox.csv')
  df.head()
 ```
 ```diff
  Date	BUDAPEST	BARANYA	BACS	BEKES	BORSOD	CSONGRAD	FEJER	GYOR	HAJDU	...	JASZ	KOMAROM	NOGRAD	PEST	SOMOGY	SZABOLCS	TOLNA	VAS	VESZPREM	ZALA
0	03/01/2005	168	79	30	173	169	42	136	120	162	...	130	57	2	178	66	64	11	29	87	68
1	10/01/2005	157	60	30	92	200	53	51	70	84	...	80	50	29	141	48	29	58	53	68	26
2	17/01/2005	96	44	31	86	93	30	93	84	191	...	64	46	4	157	33	33	24	18	62	44
3	24/01/2005	163	49	43	126	46	39	52	114	107	...	63	54	14	107	66	50	25	21	43	31
4	31/01/2005	122	78	53	87	103	34	95	131	172	...	61	49	11	124	63	56	7	47	85	60
5 rows × 21 columns
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
* Make API Call to TMDB to return the data
```python
#importing libraries
import pandas as pd
import requests 

result = requests.get('https://api.themoviedb.org/3/discover/movie?api_key=' +  api_key + '&primary_release_year=2020&sort_by=revenue.desc')
highest_revenue = response.json() # store parsed json response
highest_revenue_films = highest_revenue['results']

# define column names for our new dataframe
columns = ['film', 'revenue','original_language']
# create dataframe with film, revenue, original_language columns
df = pandas.DataFrame(columns=columns)

# make an api call for specific movie to return the budget and revenue
for film in highest_revenue_films:
    film_revenue = requests.get('https://api.themoviedb.org/3/movie/'+ str(film['id']) +'api_key')
    film_revenue = film_revenue.json()
    df_api.loc[len(df_api)]=[film['title'],film_revenue['revenue'],film['original_language']]
```    
