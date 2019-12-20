
# A program For Data Science Conference
Notebook Link: <br>
https://eu-gb.dataplatform.cloud.ibm.com/analytics/notebooks/v2/bd5593cc-fcd2-423e-90d4-235619c03b4a/view?access_token=ab81ee2cad031c0ad7360ff28ba2fe6f1fd53653d9de3a3fee880870fa3aeb4c

<hr>
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
ClIENT_ID = 'include_your_client_id' # your Foursquare ID
ClIENT_SECRET = 'include_your_client_secret' # your Foursquare Secret
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


```python

```
