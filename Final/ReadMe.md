
# A program For Data Science Conference

Introduction<br>
AutoTrader is a company based in Manchester is organizing event for 5 days for a group of Data Scientists from all over the world.The company has to put a good program, including a hotel of residence, a hall for meetings, places of landscape to visit, stores for shopping, restaurants,carparks and cafes. So the company’s purpose is to make a list of places of landscape in Manchester, including the nearest restaurants, cafes, and shopping stores for each place.<br>
<hr>
Data Description<br>
The data used in this project is provided by Foursquare location data. The data are grouped by landscape area, and each area included the information about this area and all information about restaurants, cafes, and stores which in this area.
<hr>
Table of contents<br>
1. Import Libraries<br>
2. Define Foursquare Credentials<br>
3. Define the city and get its latitude & longitude<br>
4. Search for Hotels & clean dataframe<br>
5. Search for Parking & clean dataframe<br>
6. Search for Restaurants & clean dataframe<br>
7. Search for Shopping Stores & clean dataframe<br>
8. Generate map to visualize hotels, shopping stores,Parking and Restaurant and how they cluster together<br>
<hr>

## Import Libraries


```python
import requests # to handle requests
import pandas as pd # for data analsysis
import numpy as np # to handle data in a vectorized manner

#!conda install -c conda-forge geopy --yes 
from geopy.geocoders import Nominatim # module to convert an address into latitude and longitude values

# libraries for displaying images
from IPython.display import Image 
from IPython.core.display import HTML 
    
#tranforming json file into a pandas dataframe library
from pandas.io.json import json_normalize

#!conda install -c conda-forge folium=0.5.0 --yes
import folium # plotting library
print('packages imported')

```

    packages imported


## Define Foursquare Credentials


```python
ClIENT_ID = 'A4WVHVVXOXM5QXI4I4BYJQEDSSOSZSVR5SDROS5UVZCONL0O' # your Foursquare ID
ClIENT_SECRET = '1B0N25JWRZLFAS4EIBSK4XSCAAF2H3Z0LQCDAF5PM3YBIKQA' # your Foursquare Secret
VERSION = '20180604'
LIMIT =30
print('Your credentails:')
print('Foursquare_ID: ' + ClIENT_ID)
print('Foursquare_Secret:' + ClIENT_SECRET)
```

    Your credentails:
    Foursquare_ID: A4WVHVVXOXM5QXI4I4BYJQEDSSOSZSVR5SDROS5UVZCONL0O
    Foursquare_Secret:1B0N25JWRZLFAS4EIBSK4XSCAAF2H3Z0LQCDAF5PM3YBIKQA


## Define the city and get its latitude & longitude


```python
# define the city and get its latitude & longitude 
city = 'Manchester'
geolocator = Nominatim(user_agent="foursquare_agent")
location = geolocator.geocode(city)
latitude = location.latitude
longitude = location.longitude
print(latitude, longitude)
```

    53.4794892 -2.2451148


## Hotel Search


```python
# Search hotels and get hotel dataset. 
search_query = 'Hotel'
radius = 700

# Define the corresponding URL
url = 'https://api.foursquare.com/v2/venues/search?client_id={}&client_secret={}&ll={},{}&v={}&query={}&radius={}&limit={}'\
.format(ClIENT_ID, ClIENT_SECRET, latitude, longitude, VERSION, search_query, radius, LIMIT)

# Send the GET Request and examine the results
results = requests.get(url).json()

venues = results['response']['venues']

# tranform venues into a dataframe
data = json_normalize(venues)
data.head()
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
      <th>categories</th>
      <th>hasPerk</th>
      <th>id</th>
      <th>location.address</th>
      <th>location.cc</th>
      <th>location.city</th>
      <th>location.country</th>
      <th>location.crossStreet</th>
      <th>location.distance</th>
      <th>location.formattedAddress</th>
      <th>location.labeledLatLngs</th>
      <th>location.lat</th>
      <th>location.lng</th>
      <th>location.neighborhood</th>
      <th>location.postalCode</th>
      <th>location.state</th>
      <th>name</th>
      <th>referralId</th>
      <th>venuePage.id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>4ade0dd3f964a520456d21e3</td>
      <td>Portland Street</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>511</td>
      <td>[Portland Street, Manchester, M1 4PH, United K...</td>
      <td>[{'label': 'display', 'lat': 53.47963831318294...</td>
      <td>53.479638</td>
      <td>-2.237399</td>
      <td>NaN</td>
      <td>M1 4PH</td>
      <td>NaN</td>
      <td>Mercure Manchester Piccadilly Hotel</td>
      <td>v-1576837470</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>4ade0dd9f964a520696d21e3</td>
      <td>Peter Street</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>220</td>
      <td>[Peter Street, Manchester, Greater Manchester,...</td>
      <td>[{'label': 'display', 'lat': 53.47750796558518...</td>
      <td>53.477508</td>
      <td>-2.244989</td>
      <td>NaN</td>
      <td>M60 2DS</td>
      <td>Greater Manchester</td>
      <td>The Midland Hotel</td>
      <td>v-1576837470</td>
      <td>90978518</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>4ade0ddaf964a5206f6d21e3</td>
      <td>18-24 Princess Street</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>267</td>
      <td>[18-24 Princess Street, Manchester, Greater Ma...</td>
      <td>[{'label': 'display', 'lat': 53.47824112955562...</td>
      <td>53.478241</td>
      <td>-2.241658</td>
      <td>NaN</td>
      <td>M1 4LY</td>
      <td>Greater Manchester</td>
      <td>Princess St. Hotel</td>
      <td>v-1576837470</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>4ade0dd2f964a520436d21e3</td>
      <td>Portland St</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>523</td>
      <td>[Portland St, Manchester, Greater Manchester, ...</td>
      <td>[{'label': 'display', 'lat': 53.47905633448154...</td>
      <td>53.479056</td>
      <td>-2.237239</td>
      <td>NaN</td>
      <td>M1 3LA</td>
      <td>Greater Manchester</td>
      <td>Britannia Hotel Manchester</td>
      <td>v-1576837470</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>4ade0dd3f964a520486d21e3</td>
      <td>Blackfriars St</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>510</td>
      <td>[Blackfriars St, Manchester, Greater Mancheste...</td>
      <td>[{'label': 'display', 'lat': 53.48401201209956...</td>
      <td>53.484012</td>
      <td>-2.246379</td>
      <td>NaN</td>
      <td>M3 2EQ</td>
      <td>Greater Manchester</td>
      <td>Renaissance Manchester City Centre Hotel</td>
      <td>v-1576837470</td>
      <td>71468424</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Data cleaning
clean_columns = ['name', 'categories'] + [col for col in data.columns if col.startswith('location.')]+ ['id']
clean_data = data.loc[:,clean_columns]

# function that extracts the category of the venue
def get_category_type(row):
    try:
        categories_list = row['categories']
    except:
        categories_list = row['venue.categories']
        
    if len(categories_list) == 0:
        return None
    else:
        return categories_list[0]['name']

# filter the category for each row
clean_data['categories'] = clean_data.apply(get_category_type, axis=1)

# clean column names by keeping only last term
clean_data.columns = [column.split('.')[-1] for column in clean_data.columns]

clean_data.head()
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>cc</th>
      <th>city</th>
      <th>country</th>
      <th>crossStreet</th>
      <th>distance</th>
      <th>formattedAddress</th>
      <th>labeledLatLngs</th>
      <th>lat</th>
      <th>lng</th>
      <th>neighborhood</th>
      <th>postalCode</th>
      <th>state</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mercure Manchester Piccadilly Hotel</td>
      <td>Hotel</td>
      <td>Portland Street</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>511</td>
      <td>[Portland Street, Manchester, M1 4PH, United K...</td>
      <td>[{'label': 'display', 'lat': 53.47963831318294...</td>
      <td>53.479638</td>
      <td>-2.237399</td>
      <td>NaN</td>
      <td>M1 4PH</td>
      <td>NaN</td>
      <td>4ade0dd3f964a520456d21e3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Midland Hotel</td>
      <td>Hotel</td>
      <td>Peter Street</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>220</td>
      <td>[Peter Street, Manchester, Greater Manchester,...</td>
      <td>[{'label': 'display', 'lat': 53.47750796558518...</td>
      <td>53.477508</td>
      <td>-2.244989</td>
      <td>NaN</td>
      <td>M60 2DS</td>
      <td>Greater Manchester</td>
      <td>4ade0dd9f964a520696d21e3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Princess St. Hotel</td>
      <td>Hotel</td>
      <td>18-24 Princess Street</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>267</td>
      <td>[18-24 Princess Street, Manchester, Greater Ma...</td>
      <td>[{'label': 'display', 'lat': 53.47824112955562...</td>
      <td>53.478241</td>
      <td>-2.241658</td>
      <td>NaN</td>
      <td>M1 4LY</td>
      <td>Greater Manchester</td>
      <td>4ade0ddaf964a5206f6d21e3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Britannia Hotel Manchester</td>
      <td>Hotel</td>
      <td>Portland St</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>523</td>
      <td>[Portland St, Manchester, Greater Manchester, ...</td>
      <td>[{'label': 'display', 'lat': 53.47905633448154...</td>
      <td>53.479056</td>
      <td>-2.237239</td>
      <td>NaN</td>
      <td>M1 3LA</td>
      <td>Greater Manchester</td>
      <td>4ade0dd2f964a520436d21e3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Renaissance Manchester City Centre Hotel</td>
      <td>Hotel</td>
      <td>Blackfriars St</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>510</td>
      <td>[Blackfriars St, Manchester, Greater Mancheste...</td>
      <td>[{'label': 'display', 'lat': 53.48401201209956...</td>
      <td>53.484012</td>
      <td>-2.246379</td>
      <td>NaN</td>
      <td>M3 2EQ</td>
      <td>Greater Manchester</td>
      <td>4ade0dd3f964a520486d21e3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# delete unnecessary columns
clean_data= clean_data.drop(['cc', 'city', 'country', 'crossStreet', 'distance', 'formattedAddress',\
                                        'labeledLatLngs','neighborhood', 'id'], axis=1)

clean_data.head()
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mercure Manchester Piccadilly Hotel</td>
      <td>Hotel</td>
      <td>Portland Street</td>
      <td>53.479638</td>
      <td>-2.237399</td>
      <td>M1 4PH</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Midland Hotel</td>
      <td>Hotel</td>
      <td>Peter Street</td>
      <td>53.477508</td>
      <td>-2.244989</td>
      <td>M60 2DS</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Princess St. Hotel</td>
      <td>Hotel</td>
      <td>18-24 Princess Street</td>
      <td>53.478241</td>
      <td>-2.241658</td>
      <td>M1 4LY</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Britannia Hotel Manchester</td>
      <td>Hotel</td>
      <td>Portland St</td>
      <td>53.479056</td>
      <td>-2.237239</td>
      <td>M1 3LA</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Renaissance Manchester City Centre Hotel</td>
      <td>Hotel</td>
      <td>Blackfriars St</td>
      <td>53.484012</td>
      <td>-2.246379</td>
      <td>M3 2EQ</td>
      <td>Greater Manchester</td>
    </tr>
  </tbody>
</table>
</div>




```python
# delete rows with none values (missing values)
clean_data = clean_data.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)
clean_data.head()
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>The Midland Hotel</td>
      <td>Hotel</td>
      <td>Peter Street</td>
      <td>53.477508</td>
      <td>-2.244989</td>
      <td>M60 2DS</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Princess St. Hotel</td>
      <td>Hotel</td>
      <td>18-24 Princess Street</td>
      <td>53.478241</td>
      <td>-2.241658</td>
      <td>M1 4LY</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Britannia Hotel Manchester</td>
      <td>Hotel</td>
      <td>Portland St</td>
      <td>53.479056</td>
      <td>-2.237239</td>
      <td>M1 3LA</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Renaissance Manchester City Centre Hotel</td>
      <td>Hotel</td>
      <td>Blackfriars St</td>
      <td>53.484012</td>
      <td>-2.246379</td>
      <td>M3 2EQ</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Manchester Marriott Victoria &amp; Albert Hotel</td>
      <td>Hotel</td>
      <td>Water Street</td>
      <td>53.479565</td>
      <td>-2.256742</td>
      <td>M3 4JQ</td>
      <td>Greater Manchester</td>
    </tr>
  </tbody>
</table>
</div>




```python
# delete rows which its category is not Hotel or Event Space
array= ['Hotel', 'Event Space']
hotel_data= clean_data.loc[clean_data['categories'].isin(array)]
hotel_data.head()
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>The Midland Hotel</td>
      <td>Hotel</td>
      <td>Peter Street</td>
      <td>53.477508</td>
      <td>-2.244989</td>
      <td>M60 2DS</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Princess St. Hotel</td>
      <td>Hotel</td>
      <td>18-24 Princess Street</td>
      <td>53.478241</td>
      <td>-2.241658</td>
      <td>M1 4LY</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Britannia Hotel Manchester</td>
      <td>Hotel</td>
      <td>Portland St</td>
      <td>53.479056</td>
      <td>-2.237239</td>
      <td>M1 3LA</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Renaissance Manchester City Centre Hotel</td>
      <td>Hotel</td>
      <td>Blackfriars St</td>
      <td>53.484012</td>
      <td>-2.246379</td>
      <td>M3 2EQ</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Manchester Marriott Victoria &amp; Albert Hotel</td>
      <td>Hotel</td>
      <td>Water Street</td>
      <td>53.479565</td>
      <td>-2.256742</td>
      <td>M3 4JQ</td>
      <td>Greater Manchester</td>
    </tr>
  </tbody>
</table>
</div>




```python
# delete rows which has duplicate hotel's name
print('Hotel data size:',hotel_data.shape)
hotel_data = hotel_data.drop_duplicates(subset='name', keep="first")
print('Hotel data size:',hotel_data.shape)
```

    Hotel data size: (9, 7)
    Hotel data size: (9, 7)



```python
# choose the hotel which has the same postalCode with the event space
df_hotel = hotel_data[hotel_data.postalCode == 'M1 3LA']
df_hotel
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>Britannia Hotel Manchester</td>
      <td>Hotel</td>
      <td>Portland St</td>
      <td>53.479056</td>
      <td>-2.237239</td>
      <td>M1 3LA</td>
      <td>Greater Manchester</td>
    </tr>
  </tbody>
</table>
</div>



## Parking Search


```python
# search for Parks
search_query = 'Park'
radius = 100000

# Define the corresponding URL
url = 'https://api.foursquare.com/v2/venues/search?client_id={}&client_secret={}&ll={},{}&v={}&query={}&radius={}&limit={}'\
.format(ClIENT_ID, ClIENT_SECRET, latitude, longitude, VERSION, search_query, radius, LIMIT)

# Send the GET Request and examine the results
park_results = requests.get(url).json()

# assign relevant part of JSON to venues
venues = park_results['response']['venues']

# tranform venues into a dataframe
park_data = json_normalize(venues)
park_data.head()
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
      <th>categories</th>
      <th>hasPerk</th>
      <th>id</th>
      <th>location.address</th>
      <th>location.cc</th>
      <th>location.city</th>
      <th>location.country</th>
      <th>location.crossStreet</th>
      <th>location.distance</th>
      <th>location.formattedAddress</th>
      <th>location.labeledLatLngs</th>
      <th>location.lat</th>
      <th>location.lng</th>
      <th>location.postalCode</th>
      <th>location.state</th>
      <th>name</th>
      <th>referralId</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[{'id': '4bf58dd8d48988d1fa931735', 'name': 'H...</td>
      <td>False</td>
      <td>4b5df776f964a520867629e3</td>
      <td>4 Cheetham Hill Road</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>1189</td>
      <td>[4 Cheetham Hill Road, Manchester, M4 4EW, Uni...</td>
      <td>[{'label': 'display', 'lat': 53.48993409245847...</td>
      <td>53.489934</td>
      <td>-2.241320</td>
      <td>M4 4EW</td>
      <td>NaN</td>
      <td>Park Inn by Radisson</td>
      <td>v-1576837487</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[{'id': '4c38df4de52ce0d596b336e1', 'name': 'P...</td>
      <td>False</td>
      <td>5a673109a22db74fd8af0a6b</td>
      <td>Major St</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>638</td>
      <td>[Major St, Manchester, Greater Manchester, M1,...</td>
      <td>[{'label': 'display', 'lat': 53.479008, 'lng':...</td>
      <td>53.479008</td>
      <td>-2.235512</td>
      <td>M1</td>
      <td>Greater Manchester</td>
      <td>NCP Major Street Car Park</td>
      <td>v-1576837487</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[{'id': '4c38df4de52ce0d596b336e1', 'name': 'P...</td>
      <td>False</td>
      <td>4f3cdccaa17c8a4ed1dbf1fe</td>
      <td>75 St James Street</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>325</td>
      <td>[75 St James Street, Manchester, Greater Manch...</td>
      <td>[{'label': 'display', 'lat': 53.477661, 'lng':...</td>
      <td>53.477661</td>
      <td>-2.241282</td>
      <td>M1 4BP</td>
      <td>Greater Manchester</td>
      <td>Q-Park Piazza Car Park, Manchester</td>
      <td>v-1576837487</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[{'id': '4bf58dd8d48988d163941735', 'name': 'P...</td>
      <td>False</td>
      <td>4c1cdccbb306c9288e7c64b7</td>
      <td>NaN</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>1212</td>
      <td>[Manchester, United Kingdom]</td>
      <td>[{'label': 'display', 'lat': 53.46940145799939...</td>
      <td>53.469401</td>
      <td>-2.252026</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Hulme Park</td>
      <td>v-1576837487</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[{'id': '4c38df4de52ce0d596b336e1', 'name': 'P...</td>
      <td>False</td>
      <td>51ead8ba498e323cc5fa002c</td>
      <td>Little Peter Street</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>740</td>
      <td>[Little Peter Street, Manchester, M15 5PS, Uni...</td>
      <td>[{'label': 'display', 'lat': 53.47330098803407...</td>
      <td>53.473301</td>
      <td>-2.249202</td>
      <td>M15 5PS</td>
      <td>NaN</td>
      <td>NCP Bridgewater Car Park</td>
      <td>v-1576837487</td>
    </tr>
  </tbody>
</table>
</div>




```python
# park data cleaning

# keep only columns that include venue name, and anything that is associated with location
park_clean_columns = ['name', 'categories'] + [col for col in park_data.columns if col.startswith('location.')]+ ['id']
clean_park_data = park_data.loc[:,park_clean_columns]

# function that extracts the category of the venue
def get_category_type(row):
    try:
        categories_list1 = row['categories']
    except:
        categories_list1 = row['venue.categories']
        
    if len(categories_list1) == 0:
        return None
    else:
        return categories_list1[0]['name']

# filter the category for each row
clean_park_data['categories'] = clean_park_data.apply(get_category_type, axis=1)

# clean column names by keeping only last term
clean_park_data.columns = [column.split('.')[-1] for column in clean_park_data.columns]

clean_park_data.head()

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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>cc</th>
      <th>city</th>
      <th>country</th>
      <th>crossStreet</th>
      <th>distance</th>
      <th>formattedAddress</th>
      <th>labeledLatLngs</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Park Inn by Radisson</td>
      <td>Hotel</td>
      <td>4 Cheetham Hill Road</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>1189</td>
      <td>[4 Cheetham Hill Road, Manchester, M4 4EW, Uni...</td>
      <td>[{'label': 'display', 'lat': 53.48993409245847...</td>
      <td>53.489934</td>
      <td>-2.241320</td>
      <td>M4 4EW</td>
      <td>NaN</td>
      <td>4b5df776f964a520867629e3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NCP Major Street Car Park</td>
      <td>Parking</td>
      <td>Major St</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>638</td>
      <td>[Major St, Manchester, Greater Manchester, M1,...</td>
      <td>[{'label': 'display', 'lat': 53.479008, 'lng':...</td>
      <td>53.479008</td>
      <td>-2.235512</td>
      <td>M1</td>
      <td>Greater Manchester</td>
      <td>5a673109a22db74fd8af0a6b</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Q-Park Piazza Car Park, Manchester</td>
      <td>Parking</td>
      <td>75 St James Street</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>325</td>
      <td>[75 St James Street, Manchester, Greater Manch...</td>
      <td>[{'label': 'display', 'lat': 53.477661, 'lng':...</td>
      <td>53.477661</td>
      <td>-2.241282</td>
      <td>M1 4BP</td>
      <td>Greater Manchester</td>
      <td>4f3cdccaa17c8a4ed1dbf1fe</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hulme Park</td>
      <td>Park</td>
      <td>NaN</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>1212</td>
      <td>[Manchester, United Kingdom]</td>
      <td>[{'label': 'display', 'lat': 53.46940145799939...</td>
      <td>53.469401</td>
      <td>-2.252026</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4c1cdccbb306c9288e7c64b7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NCP Bridgewater Car Park</td>
      <td>Parking</td>
      <td>Little Peter Street</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>740</td>
      <td>[Little Peter Street, Manchester, M15 5PS, Uni...</td>
      <td>[{'label': 'display', 'lat': 53.47330098803407...</td>
      <td>53.473301</td>
      <td>-2.249202</td>
      <td>M15 5PS</td>
      <td>NaN</td>
      <td>51ead8ba498e323cc5fa002c</td>
    </tr>
  </tbody>
</table>
</div>




```python
# delete unnecessary columns
clean_park_data= clean_park_data.drop(['cc', 'city', 'country', 'crossStreet', 'distance', 'formattedAddress',\
                                        'labeledLatLngs', 'id'], axis=1)

clean_park_data.head()
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Park Inn by Radisson</td>
      <td>Hotel</td>
      <td>4 Cheetham Hill Road</td>
      <td>53.489934</td>
      <td>-2.241320</td>
      <td>M4 4EW</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NCP Major Street Car Park</td>
      <td>Parking</td>
      <td>Major St</td>
      <td>53.479008</td>
      <td>-2.235512</td>
      <td>M1</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Q-Park Piazza Car Park, Manchester</td>
      <td>Parking</td>
      <td>75 St James Street</td>
      <td>53.477661</td>
      <td>-2.241282</td>
      <td>M1 4BP</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hulme Park</td>
      <td>Park</td>
      <td>NaN</td>
      <td>53.469401</td>
      <td>-2.252026</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NCP Bridgewater Car Park</td>
      <td>Parking</td>
      <td>Little Peter Street</td>
      <td>53.473301</td>
      <td>-2.249202</td>
      <td>M15 5PS</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# delete rows with none values (missing values)
clean_park_data = clean_park_data.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)
clean_park_data.head()
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>NCP Major Street Car Park</td>
      <td>Parking</td>
      <td>Major St</td>
      <td>53.479008</td>
      <td>-2.235512</td>
      <td>M1</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Q-Park Piazza Car Park, Manchester</td>
      <td>Parking</td>
      <td>75 St James Street</td>
      <td>53.477661</td>
      <td>-2.241282</td>
      <td>M1 4BP</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Q-Park Deansgate North Car Park, Manchester</td>
      <td>Parking</td>
      <td>2 Chapel Street</td>
      <td>53.485912</td>
      <td>-2.245509</td>
      <td>M3 7WJ</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Q-Park Piccadilly Place Car Park, Manchester</td>
      <td>Parking</td>
      <td>Whitworth Street</td>
      <td>53.475538</td>
      <td>-2.239354</td>
      <td>M1 3BN</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Manchester Fort Retail Park</td>
      <td>Shopping Plaza</td>
      <td>Cheetham Hill Road</td>
      <td>53.497997</td>
      <td>-2.235790</td>
      <td>M8 8EP</td>
      <td>Greater Manchester</td>
    </tr>
  </tbody>
</table>
</div>




```python
# carparks near event 
clean_park_data[(clean_park_data.postalCode.str.contains('M1 ')) &(clean_park_data.categories=='Parking')]
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Q-Park Piazza Car Park, Manchester</td>
      <td>Parking</td>
      <td>75 St James Street</td>
      <td>53.477661</td>
      <td>-2.241282</td>
      <td>M1 4BP</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Q-Park Piccadilly Place Car Park, Manchester</td>
      <td>Parking</td>
      <td>Whitworth Street</td>
      <td>53.475538</td>
      <td>-2.239354</td>
      <td>M1 3BN</td>
      <td>Greater Manchester</td>
    </tr>
  </tbody>
</table>
</div>



## Search for Restaurants 


```python
# search for Restaurants
search_query = 'Restaurant'
radius = 10000

# Define the corresponding URL
url = 'https://api.foursquare.com/v2/venues/search?client_id={}&client_secret={}&ll={},{}&v={}&query={}&radius={}&limit={}'.format(ClIENT_ID, ClIENT_SECRET, latitude, longitude, VERSION, search_query, radius, LIMIT)

Restaurant_data = requests.get(url).json()

# assign relevant part of JSON to venues
venues = Restaurant_data['response']['venues']

# tranform venues into a dataframe
Restaurant_data = json_normalize(venues)

# data cleaning 
# keep only columns that include venue name, and anything that is associated with location
Restaurant_clean_columns = ['name', 'categories'] + [col for col in Restaurant_data.columns if col.startswith('location.')]+ ['id']
clean_Restaurant_data = Restaurant_data.loc[:,Restaurant_clean_columns]

# function that extracts the category of the venue
def get_category_type(row):
    try:
        categories_list3 = row['categories']
    except:
        categories_list3 = row['venue.categories']
        
    if len(categories_list3) == 0:
        return None
    else:
        return categories_list3[0]['name']

# filter the category for each row
clean_Restaurant_data['categories'] = clean_Restaurant_data.apply(get_category_type, axis=1)

# clean column names by keeping only last term
clean_Restaurant_data.columns = [column.split('.')[-1] for column in clean_Restaurant_data.columns]

# delete unnecessary columns
clean_Restaurant_data= clean_Restaurant_data.drop(['cc', 'city', 'country', 'crossStreet', 'distance', 'formattedAddress',\
                                        'labeledLatLngs', 'neighborhood', 'id'], axis=1)

# delete rows with none values
df_Restaurant = clean_Restaurant_data.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)
df_Restaurant.head()
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>The Restaurant Bar and Grill</td>
      <td>Steakhouse</td>
      <td>14 John Dalton Street</td>
      <td>53.480527</td>
      <td>-2.247170</td>
      <td>M2 6JR</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Gino D'Acampo - My Restaurant</td>
      <td>Italian Restaurant</td>
      <td>98 Corporation St</td>
      <td>53.484844</td>
      <td>-2.242675</td>
      <td>M4 3TR</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>9</th>
      <td>The Glasshouse Bar &amp; Restaurant</td>
      <td>Bar</td>
      <td>70 Shudehill</td>
      <td>53.485774</td>
      <td>-2.236463</td>
      <td>M4 4AF</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Tae's Tavern - Caribbean Restaurant Bar Cafe</td>
      <td>Caribbean Restaurant</td>
      <td>187 - 189 Chapel Street</td>
      <td>53.483456</td>
      <td>-2.255137</td>
      <td>M3 5EQ</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wings Restaurant</td>
      <td>Chinese Restaurant</td>
      <td>Brazenose St.</td>
      <td>53.479729</td>
      <td>-2.247190</td>
      <td>M2 5LN</td>
      <td>Greater Manchester</td>
    </tr>
  </tbody>
</table>
</div>



## Search for Shopping Stores


```python
# search for Shopping
search_query = 'Shopping'
radius = 1000

# Define the corresponding URL
url = 'https://api.foursquare.com/v2/venues/search?client_id={}&client_secret={}&ll={},{}&v={}&query={}&radius={}&limit={}'.format(ClIENT_ID, ClIENT_SECRET, latitude, longitude, VERSION, search_query, radius, LIMIT)

# Send the GET Request and examine the results
sresults = requests.get(url).json()

# assign relevant part of JSON to venues
venues = sresults['response']['venues']

# tranform venues into a dataframe
Shopping_dataframe = json_normalize(venues)

# clean shopping data 
# keep only columns that include venue name, and anything that is associated with location
Shopping_clean_columns = ['name', 'categories'] + [col for col in Shopping_dataframe.columns if col.startswith('location.')]+ ['id']
clean_Shopping_dataframe = Shopping_dataframe.loc[:,Shopping_clean_columns]

# function that extracts the category of the venue
def get_category_type(row):
    try:
        categories_list5 = row['categories']
    except:
        categories_list5 = row['venue.categories']
        
    if len(categories_list5) == 0:
        return None
    else:
        return categories_list5[0]['name']

# filter the category for each row
clean_Shopping_dataframe['categories'] = clean_Shopping_dataframe.apply(get_category_type, axis=1)

# clean column names by keeping only last term
clean_Shopping_dataframe.columns = [column.split('.')[-1] for column in clean_Shopping_dataframe.columns]
clean_Shopping_dataframe.head()

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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>cc</th>
      <th>city</th>
      <th>country</th>
      <th>crossStreet</th>
      <th>distance</th>
      <th>formattedAddress</th>
      <th>labeledLatLngs</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Manchester Arndale</td>
      <td>Shopping Mall</td>
      <td>High St</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>541</td>
      <td>[High St, Manchester, M4 3AQ, United Kingdom]</td>
      <td>[{'label': 'display', 'lat': 53.48378988491711...</td>
      <td>53.483790</td>
      <td>-2.241297</td>
      <td>M4 3AQ</td>
      <td>NaN</td>
      <td>4ade0e37f964a5206a6f21e3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Royal Exchange Arcade</td>
      <td>Shopping Mall</td>
      <td>St Ann's Square</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>NaN</td>
      <td>355</td>
      <td>[St Ann's Square, Manchester, M2 7EA, United K...</td>
      <td>[{'label': 'display', 'lat': 53.48263760357169...</td>
      <td>53.482638</td>
      <td>-2.244235</td>
      <td>M2 7EA</td>
      <td>NaN</td>
      <td>4ade0e3df964a5208f6f21e3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sports Direct</td>
      <td>Sporting Goods Shop</td>
      <td>Unit MSU2, Arndale Shopping Centre</td>
      <td>GB</td>
      <td>Manchester</td>
      <td>United Kingdom</td>
      <td>Lvl 18</td>
      <td>589</td>
      <td>[Unit MSU2, Arndale Shopping Centre (Lvl 18), ...</td>
      <td>[{'label': 'display', 'lat': 53.48377034900374...</td>
      <td>53.483770</td>
      <td>-2.239871</td>
      <td>M4 2HU</td>
      <td>Greater Manchester</td>
      <td>4c111c1eb93cc9b62ecf57e0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# delete unnecessary columns
clean_Shopping_dataframe2= clean_Shopping_dataframe.drop(['cc', 'city', 'country', 'crossStreet', 'distance', 'formattedAddress',\
                                        'labeledLatLngs', 'id'], axis=1)

# delete rows which its category is not Shopping Mall
df_Shopping = clean_Shopping_dataframe2[clean_Shopping_dataframe2.categories == 'Shopping Mall']
df_Shopping.head()
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Manchester Arndale</td>
      <td>Shopping Mall</td>
      <td>High St</td>
      <td>53.483790</td>
      <td>-2.241297</td>
      <td>M4 3AQ</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Royal Exchange Arcade</td>
      <td>Shopping Mall</td>
      <td>St Ann's Square</td>
      <td>53.482638</td>
      <td>-2.244235</td>
      <td>M2 7EA</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Generate maps to visualize venues and how they cluster together


```python
# create dataframe of hotels, shopping stores and Cafeteria
hotel_neighbourhood_df = pd.concat([df_hotel, clean_park_data, df_Restaurant, df_Shopping], ignore_index=True)

hotel_neighbourhood_df = hotel_neighbourhood_df[hotel_neighbourhood_df.categories!='University']
hotel_neighbourhood_df = hotel_neighbourhood_df[hotel_neighbourhood_df.categories!='Office']
hotel_neighbourhood_df = hotel_neighbourhood_df[hotel_neighbourhood_df.categories!='College Residence Hall']
hotel_neighbourhood_df = hotel_neighbourhood_df[hotel_neighbourhood_df.categories!='Building']

hotel_neighbourhood_df
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>lat</th>
      <th>lng</th>
      <th>postalCode</th>
      <th>state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Britannia Hotel Manchester</td>
      <td>Hotel</td>
      <td>Portland St</td>
      <td>53.479056</td>
      <td>-2.237239</td>
      <td>M1 3LA</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NCP Major Street Car Park</td>
      <td>Parking</td>
      <td>Major St</td>
      <td>53.479008</td>
      <td>-2.235512</td>
      <td>M1</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Q-Park Piazza Car Park, Manchester</td>
      <td>Parking</td>
      <td>75 St James Street</td>
      <td>53.477661</td>
      <td>-2.241282</td>
      <td>M1 4BP</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Q-Park Deansgate North Car Park, Manchester</td>
      <td>Parking</td>
      <td>2 Chapel Street</td>
      <td>53.485912</td>
      <td>-2.245509</td>
      <td>M3 7WJ</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Q-Park Piccadilly Place Car Park, Manchester</td>
      <td>Parking</td>
      <td>Whitworth Street</td>
      <td>53.475538</td>
      <td>-2.239354</td>
      <td>M1 3BN</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Manchester Fort Retail Park</td>
      <td>Shopping Plaza</td>
      <td>Cheetham Hill Road</td>
      <td>53.497997</td>
      <td>-2.235790</td>
      <td>M8 8EP</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>6</th>
      <td>NCP Aquatic Cntr. Car Park</td>
      <td>Parking</td>
      <td>Booth St. East</td>
      <td>53.465915</td>
      <td>-2.232556</td>
      <td>M13 9SS</td>
      <td>Lancashire</td>
    </tr>
    <tr>
      <th>11</th>
      <td>The Restaurant Bar and Grill</td>
      <td>Steakhouse</td>
      <td>14 John Dalton Street</td>
      <td>53.480527</td>
      <td>-2.247170</td>
      <td>M2 6JR</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Gino D'Acampo - My Restaurant</td>
      <td>Italian Restaurant</td>
      <td>98 Corporation St</td>
      <td>53.484844</td>
      <td>-2.242675</td>
      <td>M4 3TR</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>13</th>
      <td>The Glasshouse Bar &amp; Restaurant</td>
      <td>Bar</td>
      <td>70 Shudehill</td>
      <td>53.485774</td>
      <td>-2.236463</td>
      <td>M4 4AF</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Tae's Tavern - Caribbean Restaurant Bar Cafe</td>
      <td>Caribbean Restaurant</td>
      <td>187 - 189 Chapel Street</td>
      <td>53.483456</td>
      <td>-2.255137</td>
      <td>M3 5EQ</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Wings Restaurant</td>
      <td>Chinese Restaurant</td>
      <td>Brazenose St.</td>
      <td>53.479729</td>
      <td>-2.247190</td>
      <td>M2 5LN</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Restaurant Mcr</td>
      <td>Restaurant</td>
      <td>Tower 12, 18-22 Bridge St</td>
      <td>53.481148</td>
      <td>-2.250515</td>
      <td>M3 3BZ</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Masons Bar Restaurant</td>
      <td>Restaurant</td>
      <td>36 Bridge Street</td>
      <td>53.481030</td>
      <td>-2.249888</td>
      <td>M3 3BT</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Restaurant</td>
      <td>Restaurant</td>
      <td>33 Simpson Street</td>
      <td>53.488416</td>
      <td>-2.234087</td>
      <td>M4 4BG</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Annyeong Korean Restaurant</td>
      <td>Korean Restaurant</td>
      <td>5-7 Chaple Walks</td>
      <td>53.481688</td>
      <td>-2.243983</td>
      <td>M2</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>20</th>
      <td>The River Bar and Restaurant</td>
      <td>Restaurant</td>
      <td>50 Dearmans Pl.</td>
      <td>53.483396</td>
      <td>-2.250650</td>
      <td>M3 5LH</td>
      <td>Salford</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Spicy City Restaurant</td>
      <td>Szechuan Restaurant</td>
      <td>56 Faulkner Street</td>
      <td>53.477987</td>
      <td>-2.240580</td>
      <td>M1 4HT</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Istanbul Restaurant Manchester</td>
      <td>Turkish Restaurant</td>
      <td>2 Bury Old Rd</td>
      <td>53.511444</td>
      <td>-2.244320</td>
      <td>M8 9JN</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>23</th>
      <td>紅餐廳 Red Restaurant</td>
      <td>Chinese Restaurant</td>
      <td>Basement,</td>
      <td>53.477352</td>
      <td>-2.240294</td>
      <td>M1 6DF</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>24</th>
      <td>China Buffet Restaurant</td>
      <td>Chinese Restaurant</td>
      <td>16 Nicholas Street</td>
      <td>53.478420</td>
      <td>-2.239957</td>
      <td>M1 4EJ</td>
      <td>Greater Manchester</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Manchester Arndale</td>
      <td>Shopping Mall</td>
      <td>High St</td>
      <td>53.483790</td>
      <td>-2.241297</td>
      <td>M4 3AQ</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Royal Exchange Arcade</td>
      <td>Shopping Mall</td>
      <td>St Ann's Square</td>
      <td>53.482638</td>
      <td>-2.244235</td>
      <td>M2 7EA</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
hotel_map = folium.Map(location=[latitude, longitude], zoom_start=15)

for lat, lng, name, categories, address in zip(hotel_neighbourhood_df['lat'], hotel_neighbourhood_df['lng'], 
                                           hotel_neighbourhood_df['name'], hotel_neighbourhood_df['categories'],\
                                               hotel_neighbourhood_df['address']):
    label = '{}, {}'.format(name, address)
    label = folium.Popup(label, parse_html=True)
    folium.CircleMarker(
        [lat, lng],
        radius=5,
        popup=label,
        color='blue',
        fill=True,
        fill_color='blue',
        fill_opacity=0.7,
        parse_html=False).add_to(hotel_map)  
    
hotel_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgCiAgICAgICAgPHNjcmlwdD4KICAgICAgICAgICAgTF9OT19UT1VDSCA9IGZhbHNlOwogICAgICAgICAgICBMX0RJU0FCTEVfM0QgPSBmYWxzZTsKICAgICAgICA8L3NjcmlwdD4KICAgIAogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjUuMS9kaXN0L2xlYWZsZXQuanMiPjwvc2NyaXB0PgogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY29kZS5qcXVlcnkuY29tL2pxdWVyeS0xLjEyLjQubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9qcy9ib290c3RyYXAubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5qcyI+PC9zY3JpcHQ+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjUuMS9kaXN0L2xlYWZsZXQuY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vYm9vdHN0cmFwLzMuMi4wL2Nzcy9ib290c3RyYXAubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLXRoZW1lLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9mb250LWF3ZXNvbWUvNC42LjMvY3NzL2ZvbnQtYXdlc29tZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuanMuY2xvdWRmbGFyZS5jb20vYWpheC9saWJzL0xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLzIuMC4yL2xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL3Jhd2Nkbi5naXRoYWNrLmNvbS9weXRob24tdmlzdWFsaXphdGlvbi9mb2xpdW0vbWFzdGVyL2ZvbGl1bS90ZW1wbGF0ZXMvbGVhZmxldC5hd2Vzb21lLnJvdGF0ZS5jc3MiLz4KICAgIDxzdHlsZT5odG1sLCBib2R5IHt3aWR0aDogMTAwJTtoZWlnaHQ6IDEwMCU7bWFyZ2luOiAwO3BhZGRpbmc6IDA7fTwvc3R5bGU+CiAgICA8c3R5bGU+I21hcCB7cG9zaXRpb246YWJzb2x1dGU7dG9wOjA7Ym90dG9tOjA7cmlnaHQ6MDtsZWZ0OjA7fTwvc3R5bGU+CiAgICAKICAgICAgICAgICAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwKICAgICAgICAgICAgICAgIGluaXRpYWwtc2NhbGU9MS4wLCBtYXhpbXVtLXNjYWxlPTEuMCwgdXNlci1zY2FsYWJsZT1ubyIgLz4KICAgICAgICAgICAgPHN0eWxlPgogICAgICAgICAgICAgICAgI21hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMiB7CiAgICAgICAgICAgICAgICAgICAgcG9zaXRpb246IHJlbGF0aXZlOwogICAgICAgICAgICAgICAgICAgIHdpZHRoOiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgbGVmdDogMC4wJTsKICAgICAgICAgICAgICAgICAgICB0b3A6IDAuMCU7CiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgIDwvc3R5bGU+CiAgICAgICAgCjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICAgICAgICAgIDxkaXYgY2xhc3M9ImZvbGl1bS1tYXAiIGlkPSJtYXBfNGE3ODlhY2EzNmMyNDY1NzhkZmRmNGU5MjQ4Y2RhYjIiID48L2Rpdj4KICAgICAgICAKPC9ib2R5Pgo8c2NyaXB0PiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFwXzRhNzg5YWNhMzZjMjQ2NTc4ZGZkZjRlOTI0OGNkYWIyID0gTC5tYXAoCiAgICAgICAgICAgICAgICAibWFwXzRhNzg5YWNhMzZjMjQ2NTc4ZGZkZjRlOTI0OGNkYWIyIiwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBjZW50ZXI6IFs1My40Nzk0ODkyLCAtMi4yNDUxMTQ4XSwKICAgICAgICAgICAgICAgICAgICBjcnM6IEwuQ1JTLkVQU0czODU3LAogICAgICAgICAgICAgICAgICAgIHpvb206IDE1LAogICAgICAgICAgICAgICAgICAgIHpvb21Db250cm9sOiB0cnVlLAogICAgICAgICAgICAgICAgICAgIHByZWZlckNhbnZhczogZmFsc2UsCiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgICk7CgogICAgICAgICAgICAKCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfZTBhZDhhNzNmODMwNGY0NTk1NDc5YjQwMDBhYTUyZDMgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICJodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZyIsCiAgICAgICAgICAgICAgICB7ImF0dHJpYnV0aW9uIjogIkRhdGEgYnkgXHUwMDI2Y29weTsgXHUwMDNjYSBocmVmPVwiaHR0cDovL29wZW5zdHJlZXRtYXAub3JnXCJcdTAwM2VPcGVuU3RyZWV0TWFwXHUwMDNjL2FcdTAwM2UsIHVuZGVyIFx1MDAzY2EgaHJlZj1cImh0dHA6Ly93d3cub3BlbnN0cmVldG1hcC5vcmcvY29weXJpZ2h0XCJcdTAwM2VPRGJMXHUwMDNjL2FcdTAwM2UuIiwgImRldGVjdFJldGluYSI6IGZhbHNlLCAibWF4TmF0aXZlWm9vbSI6IDE4LCAibWF4Wm9vbSI6IDE4LCAibWluWm9vbSI6IDAsICJub1dyYXAiOiBmYWxzZSwgIm9wYWNpdHkiOiAxLCAic3ViZG9tYWlucyI6ICJhYmMiLCAidG1zIjogZmFsc2V9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzRhNzg5YWNhMzZjMjQ2NTc4ZGZkZjRlOTI0OGNkYWIyKTsKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mMmY5NjFmYjFlMjQ0ZmIyOGZhZDc5NWMzNzM5YzUxYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUzLjQ3OTA1NjMzNDQ4MTU0LCAtMi4yMzcyMzkwNDA0MDQzNzgzXSwKICAgICAgICAgICAgICAgIHsiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsICJjb2xvciI6ICJibHVlIiwgImRhc2hBcnJheSI6IG51bGwsICJkYXNoT2Zmc2V0IjogbnVsbCwgImZpbGwiOiB0cnVlLCAiZmlsbENvbG9yIjogImJsdWUiLCAiZmlsbE9wYWNpdHkiOiAwLjcsICJmaWxsUnVsZSI6ICJldmVub2RkIiwgImxpbmVDYXAiOiAicm91bmQiLCAibGluZUpvaW4iOiAicm91bmQiLCAib3BhY2l0eSI6IDEuMCwgInJhZGl1cyI6IDUsICJzdHJva2UiOiB0cnVlLCAid2VpZ2h0IjogM30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfNGE3ODlhY2EzNmMyNDY1NzhkZmRmNGU5MjQ4Y2RhYjIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzg3NjZjOTkwY2VhMzQ1MmFiMDUzNTBjNDUwZDA1NjM4ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF8zZDAyZDk4NTg4OWM0ZDRlOGFjMGFjOWY4MDM4NDZkMyA9ICQoYDxkaXYgaWQ9Imh0bWxfM2QwMmQ5ODU4ODljNGQ0ZThhYzBhYzlmODAzODQ2ZDMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJyaXRhbm5pYSBIb3RlbCBNYW5jaGVzdGVyLCBQb3J0bGFuZCBTdDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF84NzY2Yzk5MGNlYTM0NTJhYjA1MzUwYzQ1MGQwNTYzOC5zZXRDb250ZW50KGh0bWxfM2QwMmQ5ODU4ODljNGQ0ZThhYzBhYzlmODAzODQ2ZDMpOwogICAgICAgIAoKICAgICAgICBjaXJjbGVfbWFya2VyX2YyZjk2MWZiMWUyNDRmYjI4ZmFkNzk1YzM3MzljNTFjLmJpbmRQb3B1cChwb3B1cF84NzY2Yzk5MGNlYTM0NTJhYjA1MzUwYzQ1MGQwNTYzOCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzFhNzU1NTFkYzQ3NDcxYWJhMGVlZTlhNjUyYzU4MzIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1My40NzkwMDgsIC0yLjIzNTUxMl0sCiAgICAgICAgICAgICAgICB7ImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLCAiY29sb3IiOiAiYmx1ZSIsICJkYXNoQXJyYXkiOiBudWxsLCAiZGFzaE9mZnNldCI6IG51bGwsICJmaWxsIjogdHJ1ZSwgImZpbGxDb2xvciI6ICJibHVlIiwgImZpbGxPcGFjaXR5IjogMC43LCAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsICJsaW5lQ2FwIjogInJvdW5kIiwgImxpbmVKb2luIjogInJvdW5kIiwgIm9wYWNpdHkiOiAxLjAsICJyYWRpdXMiOiA1LCAic3Ryb2tlIjogdHJ1ZSwgIndlaWdodCI6IDN9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzRhNzg5YWNhMzZjMjQ2NTc4ZGZkZjRlOTI0OGNkYWIyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9iYzA1OTdhODEzZDE0MzBkOWEwNWE1ZjhkNzUyODk2NyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNmVhZjVlNDQ0YThkNDExNjlmMTM3NmI5NjhjM2IyZTMgPSAkKGA8ZGl2IGlkPSJodG1sXzZlYWY1ZTQ0NGE4ZDQxMTY5ZjEzNzZiOTY4YzNiMmUzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OQ1AgTWFqb3IgU3RyZWV0IENhciBQYXJrLCBNYWpvciBTdDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9iYzA1OTdhODEzZDE0MzBkOWEwNWE1ZjhkNzUyODk2Ny5zZXRDb250ZW50KGh0bWxfNmVhZjVlNDQ0YThkNDExNjlmMTM3NmI5NjhjM2IyZTMpOwogICAgICAgIAoKICAgICAgICBjaXJjbGVfbWFya2VyXzcxYTc1NTUxZGM0NzQ3MWFiYTBlZWU5YTY1MmM1ODMyLmJpbmRQb3B1cChwb3B1cF9iYzA1OTdhODEzZDE0MzBkOWEwNWE1ZjhkNzUyODk2NykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZDc3MjczZjEyZTMzNDM1ODk2N2ViNzJmMzRjMGM2NjUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1My40Nzc2NjEsIC0yLjI0MTI4Ml0sCiAgICAgICAgICAgICAgICB7ImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLCAiY29sb3IiOiAiYmx1ZSIsICJkYXNoQXJyYXkiOiBudWxsLCAiZGFzaE9mZnNldCI6IG51bGwsICJmaWxsIjogdHJ1ZSwgImZpbGxDb2xvciI6ICJibHVlIiwgImZpbGxPcGFjaXR5IjogMC43LCAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsICJsaW5lQ2FwIjogInJvdW5kIiwgImxpbmVKb2luIjogInJvdW5kIiwgIm9wYWNpdHkiOiAxLjAsICJyYWRpdXMiOiA1LCAic3Ryb2tlIjogdHJ1ZSwgIndlaWdodCI6IDN9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzRhNzg5YWNhMzZjMjQ2NTc4ZGZkZjRlOTI0OGNkYWIyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8zN2VhMDY0MWJiYTc0NDFjYjI4NjM3M2RhN2Q3ZTIwOSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNmNlNDYzYjNhYTRkNDM3OGEzOWE2ZTBiY2Y4ZDU2YWUgPSAkKGA8ZGl2IGlkPSJodG1sXzZjZTQ2M2IzYWE0ZDQzNzhhMzlhNmUwYmNmOGQ1NmFlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5RLVBhcmsgUGlhenphIENhciBQYXJrLCBNYW5jaGVzdGVyLCA3NSBTdCBKYW1lcyBTdHJlZXQ8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMzdlYTA2NDFiYmE3NDQxY2IyODYzNzNkYTdkN2UyMDkuc2V0Q29udGVudChodG1sXzZjZTQ2M2IzYWE0ZDQzNzhhMzlhNmUwYmNmOGQ1NmFlKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl9kNzcyNzNmMTJlMzM0MzU4OTY3ZWI3MmYzNGMwYzY2NS5iaW5kUG9wdXAocG9wdXBfMzdlYTA2NDFiYmE3NDQxY2IyODYzNzNkYTdkN2UyMDkpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzIzM2ZiZGM4NzdiZjRiNjg5NTgwOWZiN2Y0NTYxZTk0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDg1OTEyLCAtMi4yNDU1MDldLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZDBiMWU2MzExZTg3NGQ4N2FiYjM4N2NlMWY2MDI0ZGMgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzM5NmRkMTdhNjg2MjQzZmI4NThjODI3NTNmNDQxZjg2ID0gJChgPGRpdiBpZD0iaHRtbF8zOTZkZDE3YTY4NjI0M2ZiODU4YzgyNzUzZjQ0MWY4NiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+US1QYXJrIERlYW5zZ2F0ZSBOb3J0aCBDYXIgUGFyaywgTWFuY2hlc3RlciwgMiBDaGFwZWwgU3RyZWV0PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2QwYjFlNjMxMWU4NzRkODdhYmIzODdjZTFmNjAyNGRjLnNldENvbnRlbnQoaHRtbF8zOTZkZDE3YTY4NjI0M2ZiODU4YzgyNzUzZjQ0MWY4Nik7CiAgICAgICAgCgogICAgICAgIGNpcmNsZV9tYXJrZXJfMjMzZmJkYzg3N2JmNGI2ODk1ODA5ZmI3ZjQ1NjFlOTQuYmluZFBvcHVwKHBvcHVwX2QwYjFlNjMxMWU4NzRkODdhYmIzODdjZTFmNjAyNGRjKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wM2NiYmQ0ODczZTY0NjkzYjgwNjFkYWQzMmJlMmVjNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUzLjQ3NTUzOCwgLTIuMjM5MzU0XSwKICAgICAgICAgICAgICAgIHsiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsICJjb2xvciI6ICJibHVlIiwgImRhc2hBcnJheSI6IG51bGwsICJkYXNoT2Zmc2V0IjogbnVsbCwgImZpbGwiOiB0cnVlLCAiZmlsbENvbG9yIjogImJsdWUiLCAiZmlsbE9wYWNpdHkiOiAwLjcsICJmaWxsUnVsZSI6ICJldmVub2RkIiwgImxpbmVDYXAiOiAicm91bmQiLCAibGluZUpvaW4iOiAicm91bmQiLCAib3BhY2l0eSI6IDEuMCwgInJhZGl1cyI6IDUsICJzdHJva2UiOiB0cnVlLCAid2VpZ2h0IjogM30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfNGE3ODlhY2EzNmMyNDY1NzhkZmRmNGU5MjQ4Y2RhYjIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2UyOGU4NDU5NDc3OTQ4MTNhMmViNGMzNzUzOTdhZWMxID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF80ODYxYTY1YzdhY2Y0MjBkOTg1NjNiZDIyY2VjMWYxOSA9ICQoYDxkaXYgaWQ9Imh0bWxfNDg2MWE2NWM3YWNmNDIwZDk4NTYzYmQyMmNlYzFmMTkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlEtUGFyayBQaWNjYWRpbGx5IFBsYWNlIENhciBQYXJrLCBNYW5jaGVzdGVyLCBXaGl0d29ydGggU3RyZWV0PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2UyOGU4NDU5NDc3OTQ4MTNhMmViNGMzNzUzOTdhZWMxLnNldENvbnRlbnQoaHRtbF80ODYxYTY1YzdhY2Y0MjBkOTg1NjNiZDIyY2VjMWYxOSk7CiAgICAgICAgCgogICAgICAgIGNpcmNsZV9tYXJrZXJfMDNjYmJkNDg3M2U2NDY5M2I4MDYxZGFkMzJiZTJlYzQuYmluZFBvcHVwKHBvcHVwX2UyOGU4NDU5NDc3OTQ4MTNhMmViNGMzNzUzOTdhZWMxKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82NDg0MTZhNDc4OGQ0NDkxYWMxMzJkNjdkMjA5ZDRhZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUzLjQ5Nzk5NjUwNjczODk4LCAtMi4yMzU3OTAxNzMyNDQzOTldLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZjY5ZDJlOTI3M2NjNGQ4M2IxNTJmNzhmNWQzMTMzYjQgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzNjMzliOTMyZjYyNzQ2ZTdhNWY3NTNiODdkYzA1YjZlID0gJChgPGRpdiBpZD0iaHRtbF8zYzM5YjkzMmY2Mjc0NmU3YTVmNzUzYjg3ZGMwNWI2ZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TWFuY2hlc3RlciBGb3J0IFJldGFpbCBQYXJrLCBDaGVldGhhbSBIaWxsIFJvYWQ8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZjY5ZDJlOTI3M2NjNGQ4M2IxNTJmNzhmNWQzMTMzYjQuc2V0Q29udGVudChodG1sXzNjMzliOTMyZjYyNzQ2ZTdhNWY3NTNiODdkYzA1YjZlKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl82NDg0MTZhNDc4OGQ0NDkxYWMxMzJkNjdkMjA5ZDRhZS5iaW5kUG9wdXAocG9wdXBfZjY5ZDJlOTI3M2NjNGQ4M2IxNTJmNzhmNWQzMTMzYjQpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q3MGY1NmQxYWU1MjRiNGE5MjllNWE0ZGI5OTI4NTM4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDY1OTE1NDM4MTE1NzYsIC0yLjIzMjU1NjM3MDIyODQ2N10sCiAgICAgICAgICAgICAgICB7ImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLCAiY29sb3IiOiAiYmx1ZSIsICJkYXNoQXJyYXkiOiBudWxsLCAiZGFzaE9mZnNldCI6IG51bGwsICJmaWxsIjogdHJ1ZSwgImZpbGxDb2xvciI6ICJibHVlIiwgImZpbGxPcGFjaXR5IjogMC43LCAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsICJsaW5lQ2FwIjogInJvdW5kIiwgImxpbmVKb2luIjogInJvdW5kIiwgIm9wYWNpdHkiOiAxLjAsICJyYWRpdXMiOiA1LCAic3Ryb2tlIjogdHJ1ZSwgIndlaWdodCI6IDN9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzRhNzg5YWNhMzZjMjQ2NTc4ZGZkZjRlOTI0OGNkYWIyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF82MmIyNmIyYmY3MmQ0MzU3OGRlZjJlMjYxM2MwZTRhNCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNjI2MmI2MGZlMjMwNGE4YWIyNWU0YzQ1OTgwN2QxNTMgPSAkKGA8ZGl2IGlkPSJodG1sXzYyNjJiNjBmZTIzMDRhOGFiMjVlNGM0NTk4MDdkMTUzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OQ1AgQXF1YXRpYyBDbnRyLiBDYXIgUGFyaywgQm9vdGggU3QuIEVhc3Q8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNjJiMjZiMmJmNzJkNDM1NzhkZWYyZTI2MTNjMGU0YTQuc2V0Q29udGVudChodG1sXzYyNjJiNjBmZTIzMDRhOGFiMjVlNGM0NTk4MDdkMTUzKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl9kNzBmNTZkMWFlNTI0YjRhOTI5ZTVhNGRiOTkyODUzOC5iaW5kUG9wdXAocG9wdXBfNjJiMjZiMmJmNzJkNDM1NzhkZWYyZTI2MTNjMGU0YTQpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzI5OGQyZTM1NGQ3MDQ2NTNiZmMzODc3YmVhOWUwY2IwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDgwNTI2NzM4MzA1Njg0LCAtMi4yNDcxNjk5NDk2ODA1OTg2XSwKICAgICAgICAgICAgICAgIHsiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsICJjb2xvciI6ICJibHVlIiwgImRhc2hBcnJheSI6IG51bGwsICJkYXNoT2Zmc2V0IjogbnVsbCwgImZpbGwiOiB0cnVlLCAiZmlsbENvbG9yIjogImJsdWUiLCAiZmlsbE9wYWNpdHkiOiAwLjcsICJmaWxsUnVsZSI6ICJldmVub2RkIiwgImxpbmVDYXAiOiAicm91bmQiLCAibGluZUpvaW4iOiAicm91bmQiLCAib3BhY2l0eSI6IDEuMCwgInJhZGl1cyI6IDUsICJzdHJva2UiOiB0cnVlLCAid2VpZ2h0IjogM30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfNGE3ODlhY2EzNmMyNDY1NzhkZmRmNGU5MjQ4Y2RhYjIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2U5OWIxNWUwYzk1MTQ2YTY4NjQwNjYxZGFkOTM4Y2E0ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF82M2U0NDYyOGQzZWE0NzBlODlkOWM2M2E2MWQyNGZjMSA9ICQoYDxkaXYgaWQ9Imh0bWxfNjNlNDQ2MjhkM2VhNDcwZTg5ZDljNjNhNjFkMjRmYzEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRoZSBSZXN0YXVyYW50IEJhciBhbmQgR3JpbGwsIDE0IEpvaG4gRGFsdG9uIFN0cmVldDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9lOTliMTVlMGM5NTE0NmE2ODY0MDY2MWRhZDkzOGNhNC5zZXRDb250ZW50KGh0bWxfNjNlNDQ2MjhkM2VhNDcwZTg5ZDljNjNhNjFkMjRmYzEpOwogICAgICAgIAoKICAgICAgICBjaXJjbGVfbWFya2VyXzI5OGQyZTM1NGQ3MDQ2NTNiZmMzODc3YmVhOWUwY2IwLmJpbmRQb3B1cChwb3B1cF9lOTliMTVlMGM5NTE0NmE2ODY0MDY2MWRhZDkzOGNhNCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfODJhMGE3Zjc3NDE2NDAwMDkzOWZlY2UxZGVkMTFkZjcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1My40ODQ4NDM1NTEzNDA1NzUsIC0yLjI0MjY3NDU2NDQ3NzA1NjNdLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfYzMzYTRmNjEzMDBlNDFiY2FiNDUyOTk4ODQ3NzU1MzMgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2VlMTUyMTBjMDJmNzQ5MjlhMDNmNDVkZTFmZDhhN2NiID0gJChgPGRpdiBpZD0iaHRtbF9lZTE1MjEwYzAyZjc0OTI5YTAzZjQ1ZGUxZmQ4YTdjYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+R2lubyBEJiMzOTtBY2FtcG8gLSBNeSBSZXN0YXVyYW50LCA5OCBDb3Jwb3JhdGlvbiBTdDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9jMzNhNGY2MTMwMGU0MWJjYWI0NTI5OTg4NDc3NTUzMy5zZXRDb250ZW50KGh0bWxfZWUxNTIxMGMwMmY3NDkyOWEwM2Y0NWRlMWZkOGE3Y2IpOwogICAgICAgIAoKICAgICAgICBjaXJjbGVfbWFya2VyXzgyYTBhN2Y3NzQxNjQwMDA5MzlmZWNlMWRlZDExZGY3LmJpbmRQb3B1cChwb3B1cF9jMzNhNGY2MTMwMGU0MWJjYWI0NTI5OTg4NDc3NTUzMykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWY1NjgyYjU0MzBhNDEyM2I1NWVhMmJiMDIzYmJiYjEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1My40ODU3NzM5MDg5NjQ4OCwgLTIuMjM2NDYzNDU5NTE2NDA2M10sCiAgICAgICAgICAgICAgICB7ImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLCAiY29sb3IiOiAiYmx1ZSIsICJkYXNoQXJyYXkiOiBudWxsLCAiZGFzaE9mZnNldCI6IG51bGwsICJmaWxsIjogdHJ1ZSwgImZpbGxDb2xvciI6ICJibHVlIiwgImZpbGxPcGFjaXR5IjogMC43LCAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsICJsaW5lQ2FwIjogInJvdW5kIiwgImxpbmVKb2luIjogInJvdW5kIiwgIm9wYWNpdHkiOiAxLjAsICJyYWRpdXMiOiA1LCAic3Ryb2tlIjogdHJ1ZSwgIndlaWdodCI6IDN9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzRhNzg5YWNhMzZjMjQ2NTc4ZGZkZjRlOTI0OGNkYWIyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8xZjcyZWI2ODM0Yzk0NDdjYjc5MmYwMDk4NjU0Y2Y4YSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNzJmOTAyZmE1NmU4NDBmNGFlNTg3N2VmZjk0MzljNGYgPSAkKGA8ZGl2IGlkPSJodG1sXzcyZjkwMmZhNTZlODQwZjRhZTU4NzdlZmY5NDM5YzRmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5UaGUgR2xhc3Nob3VzZSBCYXIgJmFtcDsgUmVzdGF1cmFudCwgNzAgU2h1ZGVoaWxsPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzFmNzJlYjY4MzRjOTQ0N2NiNzkyZjAwOTg2NTRjZjhhLnNldENvbnRlbnQoaHRtbF83MmY5MDJmYTU2ZTg0MGY0YWU1ODc3ZWZmOTQzOWM0Zik7CiAgICAgICAgCgogICAgICAgIGNpcmNsZV9tYXJrZXJfMWY1NjgyYjU0MzBhNDEyM2I1NWVhMmJiMDIzYmJiYjEuYmluZFBvcHVwKHBvcHVwXzFmNzJlYjY4MzRjOTQ0N2NiNzkyZjAwOTg2NTRjZjhhKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9kOThkM2M0YTc3ODM0MTUwYWMzMzBiZGJlYjQxMDBjMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUzLjQ4MzQ1NTUxMTMzNzE3LCAtMi4yNTUxMzY5NjY3MDUzMjIzXSwKICAgICAgICAgICAgICAgIHsiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsICJjb2xvciI6ICJibHVlIiwgImRhc2hBcnJheSI6IG51bGwsICJkYXNoT2Zmc2V0IjogbnVsbCwgImZpbGwiOiB0cnVlLCAiZmlsbENvbG9yIjogImJsdWUiLCAiZmlsbE9wYWNpdHkiOiAwLjcsICJmaWxsUnVsZSI6ICJldmVub2RkIiwgImxpbmVDYXAiOiAicm91bmQiLCAibGluZUpvaW4iOiAicm91bmQiLCAib3BhY2l0eSI6IDEuMCwgInJhZGl1cyI6IDUsICJzdHJva2UiOiB0cnVlLCAid2VpZ2h0IjogM30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfNGE3ODlhY2EzNmMyNDY1NzhkZmRmNGU5MjQ4Y2RhYjIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzE0YTVmOTkwYmM2NjQzZWZiODVjYzE3ZTc1YjZjZjZlID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9iY2JiZGE3ZWMxNjI0N2NjYmNmODRiMjI1YzNlZDE1MSA9ICQoYDxkaXYgaWQ9Imh0bWxfYmNiYmRhN2VjMTYyNDdjY2JjZjg0YjIyNWMzZWQxNTEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRhZSYjMzk7cyBUYXZlcm4gLSBDYXJpYmJlYW4gUmVzdGF1cmFudCBCYXIgQ2FmZSwgMTg3IC0gMTg5IENoYXBlbCBTdHJlZXQ8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMTRhNWY5OTBiYzY2NDNlZmI4NWNjMTdlNzViNmNmNmUuc2V0Q29udGVudChodG1sX2JjYmJkYTdlYzE2MjQ3Y2NiY2Y4NGIyMjVjM2VkMTUxKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl9kOThkM2M0YTc3ODM0MTUwYWMzMzBiZGJlYjQxMDBjMS5iaW5kUG9wdXAocG9wdXBfMTRhNWY5OTBiYzY2NDNlZmI4NWNjMTdlNzViNmNmNmUpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzJkNDE0M2NjNWQxNzRjNTQ5ZTIwYjlmMWMzYmQ2NTUxID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDc5NzI4ODc4NTIxMDMsIC0yLjI0NzE4OTY5NDkyNjUyOThdLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZWJmMzM5NzhiZjE0NDk1NmIwODQ3Njk1NmEzNjc5NzEgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2FiZDg0MmFlMWQyNTRkYzE4MTcwMjJlZWQzNDA5YTBkID0gJChgPGRpdiBpZD0iaHRtbF9hYmQ4NDJhZTFkMjU0ZGMxODE3MDIyZWVkMzQwOWEwZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2luZ3MgUmVzdGF1cmFudCwgQnJhemVub3NlIFN0LjwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9lYmYzMzk3OGJmMTQ0OTU2YjA4NDc2OTU2YTM2Nzk3MS5zZXRDb250ZW50KGh0bWxfYWJkODQyYWUxZDI1NGRjMTgxNzAyMmVlZDM0MDlhMGQpOwogICAgICAgIAoKICAgICAgICBjaXJjbGVfbWFya2VyXzJkNDE0M2NjNWQxNzRjNTQ5ZTIwYjlmMWMzYmQ2NTUxLmJpbmRQb3B1cChwb3B1cF9lYmYzMzk3OGJmMTQ0OTU2YjA4NDc2OTU2YTM2Nzk3MSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzEwMzc1NmI2NDgxNGZmZWFlOGE3OTBhYmI0OTE0MjYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1My40ODExNDgsIC0yLjI1MDUxNTJdLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfYmQ5ZGJmODkxMDI0NDg2Njk3OTgyZDgzNTA2NmU4YjcgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzNmNmQ0MTliZTUzNjQwOWVhMWUyYjgzNGJjOWQzNzVlID0gJChgPGRpdiBpZD0iaHRtbF8zZjZkNDE5YmU1MzY0MDllYTFlMmI4MzRiYzlkMzc1ZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UmVzdGF1cmFudCBNY3IsIFRvd2VyIDEyLCAxOC0yMiBCcmlkZ2UgU3Q8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfYmQ5ZGJmODkxMDI0NDg2Njk3OTgyZDgzNTA2NmU4Yjcuc2V0Q29udGVudChodG1sXzNmNmQ0MTliZTUzNjQwOWVhMWUyYjgzNGJjOWQzNzVlKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl83MTAzNzU2YjY0ODE0ZmZlYWU4YTc5MGFiYjQ5MTQyNi5iaW5kUG9wdXAocG9wdXBfYmQ5ZGJmODkxMDI0NDg2Njk3OTgyZDgzNTA2NmU4YjcpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2VhOGY3MzExY2FiMDQwZjQ5NTVhNTdhNDBjZTVkNGM3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDgxMDMsIC0yLjI0OTg4OF0sCiAgICAgICAgICAgICAgICB7ImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLCAiY29sb3IiOiAiYmx1ZSIsICJkYXNoQXJyYXkiOiBudWxsLCAiZGFzaE9mZnNldCI6IG51bGwsICJmaWxsIjogdHJ1ZSwgImZpbGxDb2xvciI6ICJibHVlIiwgImZpbGxPcGFjaXR5IjogMC43LCAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsICJsaW5lQ2FwIjogInJvdW5kIiwgImxpbmVKb2luIjogInJvdW5kIiwgIm9wYWNpdHkiOiAxLjAsICJyYWRpdXMiOiA1LCAic3Ryb2tlIjogdHJ1ZSwgIndlaWdodCI6IDN9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzRhNzg5YWNhMzZjMjQ2NTc4ZGZkZjRlOTI0OGNkYWIyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8zNjc0NjI1NmVhZjk0MjVlYmUwMzgyYTIxNDk0MzQ3YSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYTFlMmM2ZWJmOGE5NDhjZjg2MDRhNTdkMThmNDFhNGUgPSAkKGA8ZGl2IGlkPSJodG1sX2ExZTJjNmViZjhhOTQ4Y2Y4NjA0YTU3ZDE4ZjQxYTRlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5NYXNvbnMgQmFyIFJlc3RhdXJhbnQsIDM2IEJyaWRnZSBTdHJlZXQ8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMzY3NDYyNTZlYWY5NDI1ZWJlMDM4MmEyMTQ5NDM0N2Euc2V0Q29udGVudChodG1sX2ExZTJjNmViZjhhOTQ4Y2Y4NjA0YTU3ZDE4ZjQxYTRlKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl9lYThmNzMxMWNhYjA0MGY0OTU1YTU3YTQwY2U1ZDRjNy5iaW5kUG9wdXAocG9wdXBfMzY3NDYyNTZlYWY5NDI1ZWJlMDM4MmEyMTQ5NDM0N2EpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2E4ODQ5MzZkODVkNTRhMTRiZWUwMDVmNDQ5NTc0MTJiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDg4NDE2LCAtMi4yMzQwODddLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZWNlNzBmNTdhNTM4NGUxYmE5NzEwY2JkNjUyYTQ2OWMgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzdlZjliMDdmY2YwNzQwMzU4OTMyYzliNTc0NzA3MTQzID0gJChgPGRpdiBpZD0iaHRtbF83ZWY5YjA3ZmNmMDc0MDM1ODkzMmM5YjU3NDcwNzE0MyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UmVzdGF1cmFudCwgMzMgU2ltcHNvbiBTdHJlZXQ8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZWNlNzBmNTdhNTM4NGUxYmE5NzEwY2JkNjUyYTQ2OWMuc2V0Q29udGVudChodG1sXzdlZjliMDdmY2YwNzQwMzU4OTMyYzliNTc0NzA3MTQzKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl9hODg0OTM2ZDg1ZDU0YTE0YmVlMDA1ZjQ0OTU3NDEyYi5iaW5kUG9wdXAocG9wdXBfZWNlNzBmNTdhNTM4NGUxYmE5NzEwY2JkNjUyYTQ2OWMpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzIzNzkxZmMyNGU1YjQ4ZTBiZjgzMDY3NGU2MTdlNzliID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDgxNjg4LCAtMi4yNDM5ODNdLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMGU3Yjg0OGM0M2ViNGQ3YWI1NDY1OWIxZGMwZjE3ZmEgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzIxZGNjOTNkNTkwMjQ4NmM5Njg2ZjVmN2U0NmU2NDVkID0gJChgPGRpdiBpZD0iaHRtbF8yMWRjYzkzZDU5MDI0ODZjOTY4NmY1ZjdlNDZlNjQ1ZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QW5ueWVvbmcgS29yZWFuIFJlc3RhdXJhbnQsIDUtNyBDaGFwbGUgV2Fsa3M8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMGU3Yjg0OGM0M2ViNGQ3YWI1NDY1OWIxZGMwZjE3ZmEuc2V0Q29udGVudChodG1sXzIxZGNjOTNkNTkwMjQ4NmM5Njg2ZjVmN2U0NmU2NDVkKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl8yMzc5MWZjMjRlNWI0OGUwYmY4MzA2NzRlNjE3ZTc5Yi5iaW5kUG9wdXAocG9wdXBfMGU3Yjg0OGM0M2ViNGQ3YWI1NDY1OWIxZGMwZjE3ZmEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzAxMGU3OTQ3OWZlMTRlNzlhNzg5ZTlkN2QyMDBjZDA2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDgzMzk1OTUyNzc0Mjk1LCAtMi4yNTA2NTAxODQ1ODI4ODFdLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfN2QxZjYyOWE2MDZjNDBlN2JmMzI4NDI4NjZkZDc3NmQgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzI3Y2U4YWIyOGNkZDQ1N2U5YWY2ZDE5OGM1MWZjMGYxID0gJChgPGRpdiBpZD0iaHRtbF8yN2NlOGFiMjhjZGQ0NTdlOWFmNmQxOThjNTFmYzBmMSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+VGhlIFJpdmVyIEJhciBhbmQgUmVzdGF1cmFudCwgNTAgRGVhcm1hbnMgUGwuPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzdkMWY2MjlhNjA2YzQwZTdiZjMyODQyODY2ZGQ3NzZkLnNldENvbnRlbnQoaHRtbF8yN2NlOGFiMjhjZGQ0NTdlOWFmNmQxOThjNTFmYzBmMSk7CiAgICAgICAgCgogICAgICAgIGNpcmNsZV9tYXJrZXJfMDEwZTc5NDc5ZmUxNGU3OWE3ODllOWQ3ZDIwMGNkMDYuYmluZFBvcHVwKHBvcHVwXzdkMWY2MjlhNjA2YzQwZTdiZjMyODQyODY2ZGQ3NzZkKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81YTg1OWUyZDQ4NTM0YzU4YjE5NzAwMGJiODkzYjNhMiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUzLjQ3Nzk4NywgLTIuMjQwNThdLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMzM3NmE5ZjNjN2EyNDIyOGE0YzIxMWJiMDI2ZTc1MmUgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2Y3Y2EwNmQxYzc2YzQ4MTY5MjgyNThiNDRlMmM0MWJlID0gJChgPGRpdiBpZD0iaHRtbF9mN2NhMDZkMWM3NmM0ODE2OTI4MjU4YjQ0ZTJjNDFiZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3BpY3kgQ2l0eSBSZXN0YXVyYW50LCA1NiBGYXVsa25lciBTdHJlZXQ8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMzM3NmE5ZjNjN2EyNDIyOGE0YzIxMWJiMDI2ZTc1MmUuc2V0Q29udGVudChodG1sX2Y3Y2EwNmQxYzc2YzQ4MTY5MjgyNThiNDRlMmM0MWJlKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl81YTg1OWUyZDQ4NTM0YzU4YjE5NzAwMGJiODkzYjNhMi5iaW5kUG9wdXAocG9wdXBfMzM3NmE5ZjNjN2EyNDIyOGE0YzIxMWJiMDI2ZTc1MmUpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2RjM2ViM2IwMzgwZDQ4MDBiZjI4NmNlM2MyMTc3YmEzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNTExNDQ0MywgLTIuMjQ0MzE5OF0sCiAgICAgICAgICAgICAgICB7ImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLCAiY29sb3IiOiAiYmx1ZSIsICJkYXNoQXJyYXkiOiBudWxsLCAiZGFzaE9mZnNldCI6IG51bGwsICJmaWxsIjogdHJ1ZSwgImZpbGxDb2xvciI6ICJibHVlIiwgImZpbGxPcGFjaXR5IjogMC43LCAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsICJsaW5lQ2FwIjogInJvdW5kIiwgImxpbmVKb2luIjogInJvdW5kIiwgIm9wYWNpdHkiOiAxLjAsICJyYWRpdXMiOiA1LCAic3Ryb2tlIjogdHJ1ZSwgIndlaWdodCI6IDN9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzRhNzg5YWNhMzZjMjQ2NTc4ZGZkZjRlOTI0OGNkYWIyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9jN2JiMWYzYzg0ODc0YTczOTBjZTg0N2VjMDA5OWQ1MSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYWU1MWY4YjA0Y2QyNDg4NjgzY2EzZjY0NTMzZTQ5YTIgPSAkKGA8ZGl2IGlkPSJodG1sX2FlNTFmOGIwNGNkMjQ4ODY4M2NhM2Y2NDUzM2U0OWEyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Jc3RhbmJ1bCBSZXN0YXVyYW50IE1hbmNoZXN0ZXIsIDIgQnVyeSBPbGQgUmQ8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfYzdiYjFmM2M4NDg3NGE3MzkwY2U4NDdlYzAwOTlkNTEuc2V0Q29udGVudChodG1sX2FlNTFmOGIwNGNkMjQ4ODY4M2NhM2Y2NDUzM2U0OWEyKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl9kYzNlYjNiMDM4MGQ0ODAwYmYyODZjZTNjMjE3N2JhMy5iaW5kUG9wdXAocG9wdXBfYzdiYjFmM2M4NDg3NGE3MzkwY2U4NDdlYzAwOTlkNTEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzg3MjhmYTA0M2UwZDRiMTM5MTc5YWYwNTE1NDU4NWRhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDc3MzUyLCAtMi4yNDAyOTRdLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfOTM4MDZlZmUyYWYzNGU4MzkyMGNjNGNmNWVlOGZlZGUgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzQ5NjNiYmU5ZjVlNjQxODhhZTY4OTI0Y2MzNTBmNWYzID0gJChgPGRpdiBpZD0iaHRtbF80OTYzYmJlOWY1ZTY0MTg4YWU2ODkyNGNjMzUwZjVmMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+57SF6aSQ5buzIFJlZCBSZXN0YXVyYW50LCBCYXNlbWVudCw8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfOTM4MDZlZmUyYWYzNGU4MzkyMGNjNGNmNWVlOGZlZGUuc2V0Q29udGVudChodG1sXzQ5NjNiYmU5ZjVlNjQxODhhZTY4OTI0Y2MzNTBmNWYzKTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl84NzI4ZmEwNDNlMGQ0YjEzOTE3OWFmMDUxNTQ1ODVkYS5iaW5kUG9wdXAocG9wdXBfOTM4MDZlZmUyYWYzNGU4MzkyMGNjNGNmNWVlOGZlZGUpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzI0NDA1NDc3NmQzNDQ5YjhiY2UzY2UyODA3NTJmYjliID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDc4NDE5NjIzMjY1ODUsIC0yLjIzOTk1NzE1NTg2MTk5NTNdLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMWNjNmM2MDI2NmIzNGFjMWI2MzY3OGQ3NTViODYyM2MgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzk3MjBmZDQyMzJjNTQyOGU4MmY0MWE5OGQ4ZWE5MmFjID0gJChgPGRpdiBpZD0iaHRtbF85NzIwZmQ0MjMyYzU0MjhlODJmNDFhOThkOGVhOTJhYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2hpbmEgQnVmZmV0IFJlc3RhdXJhbnQsIDE2IE5pY2hvbGFzIFN0cmVldDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8xY2M2YzYwMjY2YjM0YWMxYjYzNjc4ZDc1NWI4NjIzYy5zZXRDb250ZW50KGh0bWxfOTcyMGZkNDIzMmM1NDI4ZTgyZjQxYTk4ZDhlYTkyYWMpOwogICAgICAgIAoKICAgICAgICBjaXJjbGVfbWFya2VyXzI0NDA1NDc3NmQzNDQ5YjhiY2UzY2UyODA3NTJmYjliLmJpbmRQb3B1cChwb3B1cF8xY2M2YzYwMjY2YjM0YWMxYjYzNjc4ZDc1NWI4NjIzYykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDdjNjYzYjdmOWQ4NDE5OTg0MzFjYWZhYzNkNTk3NzEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1My40ODM3ODk4ODQ5MTcxMSwgLTIuMjQxMjk2NzY4MTg4NDc2Nl0sCiAgICAgICAgICAgICAgICB7ImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLCAiY29sb3IiOiAiYmx1ZSIsICJkYXNoQXJyYXkiOiBudWxsLCAiZGFzaE9mZnNldCI6IG51bGwsICJmaWxsIjogdHJ1ZSwgImZpbGxDb2xvciI6ICJibHVlIiwgImZpbGxPcGFjaXR5IjogMC43LCAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsICJsaW5lQ2FwIjogInJvdW5kIiwgImxpbmVKb2luIjogInJvdW5kIiwgIm9wYWNpdHkiOiAxLjAsICJyYWRpdXMiOiA1LCAic3Ryb2tlIjogdHJ1ZSwgIndlaWdodCI6IDN9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzRhNzg5YWNhMzZjMjQ2NTc4ZGZkZjRlOTI0OGNkYWIyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF81MDY4YmQyMjQzNmI0MDZkYTcxZGRjZGY0OGQ2YzQ3NyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYmYzMzNmMjQ2Mzc4NDMyMWJlN2NmNjg5NDQyMTg3ZDkgPSAkKGA8ZGl2IGlkPSJodG1sX2JmMzMzZjI0NjM3ODQzMjFiZTdjZjY4OTQ0MjE4N2Q5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5NYW5jaGVzdGVyIEFybmRhbGUsIEhpZ2ggU3Q8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNTA2OGJkMjI0MzZiNDA2ZGE3MWRkY2RmNDhkNmM0Nzcuc2V0Q29udGVudChodG1sX2JmMzMzZjI0NjM3ODQzMjFiZTdjZjY4OTQ0MjE4N2Q5KTsKICAgICAgICAKCiAgICAgICAgY2lyY2xlX21hcmtlcl8wN2M2NjNiN2Y5ZDg0MTk5ODQzMWNhZmFjM2Q1OTc3MS5iaW5kUG9wdXAocG9wdXBfNTA2OGJkMjI0MzZiNDA2ZGE3MWRkY2RmNDhkNmM0NzcpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzAzYzgwZGQ4YmVhZDQyNWNiNTQ5YjZkNzhmMTZlNzBhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTMuNDgyNjM3NjAzNTcxNjk2LCAtMi4yNDQyMzQ1NjAxNDA0MDFdLAogICAgICAgICAgICAgICAgeyJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwgImNvbG9yIjogImJsdWUiLCAiZGFzaEFycmF5IjogbnVsbCwgImRhc2hPZmZzZXQiOiBudWxsLCAiZmlsbCI6IHRydWUsICJmaWxsQ29sb3IiOiAiYmx1ZSIsICJmaWxsT3BhY2l0eSI6IDAuNywgImZpbGxSdWxlIjogImV2ZW5vZGQiLCAibGluZUNhcCI6ICJyb3VuZCIsICJsaW5lSm9pbiI6ICJyb3VuZCIsICJvcGFjaXR5IjogMS4wLCAicmFkaXVzIjogNSwgInN0cm9rZSI6IHRydWUsICJ3ZWlnaHQiOiAzfQogICAgICAgICAgICApLmFkZFRvKG1hcF80YTc4OWFjYTM2YzI0NjU3OGRmZGY0ZTkyNDhjZGFiMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfYWMyNzRhZTdmYmU1NGM4ODliNjk4MGI4NGI5ZGRmZmYgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2E3NmRiNmRiYWM3ZTQ3MTdiODJmMmFhNjcyYzk0OWYzID0gJChgPGRpdiBpZD0iaHRtbF9hNzZkYjZkYmFjN2U0NzE3YjgyZjJhYTY3MmM5NDlmMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Um95YWwgRXhjaGFuZ2UgQXJjYWRlLCBTdCBBbm4mIzM5O3MgU3F1YXJlPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2FjMjc0YWU3ZmJlNTRjODg5YjY5ODBiODRiOWRkZmZmLnNldENvbnRlbnQoaHRtbF9hNzZkYjZkYmFjN2U0NzE3YjgyZjJhYTY3MmM5NDlmMyk7CiAgICAgICAgCgogICAgICAgIGNpcmNsZV9tYXJrZXJfMDNjODBkZDhiZWFkNDI1Y2I1NDliNmQ3OGYxNmU3MGEuYmluZFBvcHVwKHBvcHVwX2FjMjc0YWU3ZmJlNTRjODg5YjY5ODBiODRiOWRkZmZmKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKPC9zY3JpcHQ+" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python

```
