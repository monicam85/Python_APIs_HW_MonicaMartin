

```python
#
import json
import requests as req
from citipy import citipy
import pandas as pd
import matplotlib.pyplot as plt
from datetime import date
import seaborn as sns
import numpy as np
```


```python
#%%
base_url = 'http://api.openweathermap.org/data/2.5/weather'
APIKEY = '3b6fc998272abf055734591058b4c655'
#%%
```


```python
__cities = []
query_units = 'imperial'
weather_info = []
__max_temp = []
__humidity = []
__wind_speed = []
__lon = []
__lat = []
__cloudiness  = []
__qcities  = []
__country = []
__date = []
```

### Generate Cities List


```python
#Generate city from random geographical coordinates
num_query=1500
name=[]
cod=[]
lt=180 * np.random.random_sample((num_query)) - 90 #latitude [-90, 90)
lg=360 * np.random.random_sample((num_query)) - 180 #longtitude [-180, 180)
geo_coord=list(zip(lt,lg))

for lat, lon in geo_coord:
    city=citipy.nearest_city(lat, lon)
    name.append(city.city_name)
    cod.append(city.country_code)

__qcities=list(zip(name, cod))
__qcities=list(set(__qcities)) #unique cities and country codes
len(__qcities)
```




    609



### Perform API Calls


```python
x=0
print('\n Beginning Data Retrieval\n'+'-'*26)
for qcity, qcd in __qcities:

    # Perform API Call
    query_url = base_url + '?apikey=' + APIKEY + \
        '&q=' + qcity + ',' + qcd + '&units=' + query_units
    weather_info.append(req.get(query_url).json())

    try:
        __cities.append(weather_info[x]['name'])
        rcity=qcity
    except KeyError:
        print('City of %s in %s not found' % (qcity.title(), qcd.upper()))
        #citipy error handler
        #pass
    else:
        print('Processing Record %s | %s '  % (x, rcity.title() ))
        print(query_url)
        __max_temp.append(weather_info[x]['main']['temp_max'])
        __humidity.append(weather_info[x]['main']['humidity'])
        __wind_speed.append(weather_info[x]['wind']['speed'])
        __lon.append(weather_info[x]['coord']['lon'])
        __lat.append(weather_info[x]['coord']['lat'])
        __cloudiness.append(weather_info[x]['clouds']['all'])
        __country.append(weather_info[x]['sys']['country'])
        __date.append(weather_info[x]['dt'])
    
    x+=1

print('\n'+ '-'*28+'\n Data Retrieval complete\n'+'-'*28)
weather_info = {"Temperature": __max_temp, 
                "Humidity": __humidity,
                "Wind Speed": __wind_speed,
                'Lat': __lat,
                'Lng': __lon,
                'Cloudiness': __cloudiness,
                'City': __cities,
                'Country': __country,
                'Date': __date}

weather_info = pd.DataFrame(weather_info)
```

    
     Beginning Data Retrieval
    --------------------------
    Processing Record 0 | Roald 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=roald,no&units=imperial
    Processing Record 1 | Benito Juarez 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=benito juarez,mx&units=imperial
    Processing Record 2 | Dioknisi 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=dioknisi,ge&units=imperial
    City of Meyungs in PW not found
    Processing Record 4 | Cururupu 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cururupu,br&units=imperial
    Processing Record 5 | Fort Nelson 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=fort nelson,ca&units=imperial
    Processing Record 6 | Tuktoyaktuk 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tuktoyaktuk,ca&units=imperial
    Processing Record 7 | Senador Canedo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=senador canedo,br&units=imperial
    Processing Record 8 | Maldonado 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=maldonado,uy&units=imperial
    Processing Record 9 | San Patricio 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=san patricio,mx&units=imperial
    Processing Record 10 | Adrar 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=adrar,dz&units=imperial
    Processing Record 11 | Longyearbyen 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=longyearbyen,sj&units=imperial
    City of Gogrial in SD not found
    Processing Record 13 | Kang 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kang,bw&units=imperial
    Processing Record 14 | Raudeberg 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=raudeberg,no&units=imperial
    Processing Record 15 | Jagatsinghapur 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=jagatsinghapur,in&units=imperial
    Processing Record 16 | Bambous Virieux 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bambous virieux,mu&units=imperial
    Processing Record 17 | Ponta Do Sol 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ponta do sol,cv&units=imperial
    Processing Record 18 | Port Barton 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=port barton,ph&units=imperial
    Processing Record 19 | Hit 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hit,iq&units=imperial
    Processing Record 20 | Povenets 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=povenets,ru&units=imperial
    Processing Record 21 | Atar 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=atar,mr&units=imperial
    Processing Record 22 | Ust-Omchug 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ust-omchug,ru&units=imperial
    Processing Record 23 | New Norfolk 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=new norfolk,au&units=imperial
    Processing Record 24 | Basavakalyan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=basavakalyan,in&units=imperial
    Processing Record 25 | Skibbereen 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=skibbereen,ie&units=imperial
    Processing Record 26 | Kiruna 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kiruna,se&units=imperial
    Processing Record 27 | Pitangui 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pitangui,br&units=imperial
    Processing Record 28 | Busselton 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=busselton,au&units=imperial
    Processing Record 29 | Moron 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=moron,mn&units=imperial
    Processing Record 30 | Barao De Melgaco 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=barao de melgaco,br&units=imperial
    Processing Record 31 | El Campo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=el campo,us&units=imperial
    Processing Record 32 | Umba 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=umba,ru&units=imperial
    Processing Record 33 | Komsomolskiy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=komsomolskiy,ru&units=imperial
    City of Masjed-E Soleyman in IR not found
    Processing Record 35 | Brae 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=brae,gb&units=imperial
    Processing Record 36 | Kaitangata 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kaitangata,nz&units=imperial
    Processing Record 37 | Novikovo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=novikovo,ru&units=imperial
    Processing Record 38 | Krasnozerskoye 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=krasnozerskoye,ru&units=imperial
    Processing Record 39 | Kjollefjord 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kjollefjord,no&units=imperial
    Processing Record 40 | Statesboro 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=statesboro,us&units=imperial
    Processing Record 41 | Hay River 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hay river,ca&units=imperial
    Processing Record 42 | Okhotsk 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=okhotsk,ru&units=imperial
    Processing Record 43 | Hermanus 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hermanus,za&units=imperial
    Processing Record 44 | Bolu 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bolu,tr&units=imperial
    Processing Record 45 | Saint-Francois 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=saint-francois,gp&units=imperial
    Processing Record 46 | Zhangye 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=zhangye,cn&units=imperial
    Processing Record 47 | Kavieng 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kavieng,pg&units=imperial
    Processing Record 48 | Valparaiso 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=valparaiso,cl&units=imperial
    Processing Record 49 | Tanete 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tanete,id&units=imperial
    Processing Record 50 | Moroni 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=moroni,km&units=imperial
    Processing Record 51 | Pizarro 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pizarro,co&units=imperial
    Processing Record 52 | Katangli 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=katangli,ru&units=imperial
    Processing Record 53 | Westport 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=westport,nz&units=imperial
    Processing Record 54 | Opuwo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=opuwo,na&units=imperial
    Processing Record 55 | Barrow 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=barrow,us&units=imperial
    Processing Record 56 | Awbari 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=awbari,ly&units=imperial
    Processing Record 57 | Luderitz 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=luderitz,na&units=imperial
    Processing Record 58 | Nguruka 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=nguruka,tz&units=imperial
    Processing Record 59 | Ilawa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ilawa,pl&units=imperial
    Processing Record 60 | Sabang 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sabang,id&units=imperial
    Processing Record 61 | Xingyi 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=xingyi,cn&units=imperial
    Processing Record 62 | Nikolskoye 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=nikolskoye,ru&units=imperial
    Processing Record 63 | Rocha 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=rocha,uy&units=imperial
    City of Faya in TD not found
    Processing Record 65 | Walvis Bay 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=walvis bay,na&units=imperial
    City of Asau in TV not found
    Processing Record 67 | Puerto Madryn 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=puerto madryn,ar&units=imperial
    Processing Record 68 | Jeremoabo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=jeremoabo,br&units=imperial
    Processing Record 69 | Shache 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=shache,cn&units=imperial
    Processing Record 70 | Omsukchan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=omsukchan,ru&units=imperial
    Processing Record 71 | Anshun 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=anshun,cn&units=imperial
    City of Mataura in PF not found
    Processing Record 73 | Port Alfred 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=port alfred,za&units=imperial
    Processing Record 74 | Saint-Michel-Des-Saints 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=saint-michel-des-saints,ca&units=imperial
    Processing Record 75 | Nanortalik 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=nanortalik,gl&units=imperial
    Processing Record 76 | Sorgun 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sorgun,tr&units=imperial
    Processing Record 77 | Cabo San Lucas 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cabo san lucas,mx&units=imperial
    Processing Record 78 | Torbay 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=torbay,ca&units=imperial
    Processing Record 79 | Phalaborwa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=phalaborwa,za&units=imperial
    Processing Record 80 | Maniitsoq 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=maniitsoq,gl&units=imperial
    Processing Record 81 | Itarema 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=itarema,br&units=imperial
    Processing Record 82 | Dingle 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=dingle,ie&units=imperial
    Processing Record 83 | Mindelo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mindelo,cv&units=imperial
    Processing Record 84 | Kruisfontein 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kruisfontein,za&units=imperial
    Processing Record 85 | Chapais 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=chapais,ca&units=imperial
    Processing Record 86 | Mahebourg 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mahebourg,mu&units=imperial
    Processing Record 87 | Nouadhibou 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=nouadhibou,mr&units=imperial
    Processing Record 88 | Ribeira Brava 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ribeira brava,pt&units=imperial
    Processing Record 89 | Heihe 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=heihe,cn&units=imperial
    Processing Record 90 | Marinette 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=marinette,us&units=imperial
    Processing Record 91 | Chuy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=chuy,uy&units=imperial
    Processing Record 92 | Coquimbo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=coquimbo,cl&units=imperial
    Processing Record 93 | Saskylakh 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=saskylakh,ru&units=imperial
    Processing Record 94 | Tanout 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tanout,ne&units=imperial
    City of Macaboboni in PH not found
    Processing Record 96 | Grindavik 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=grindavik,is&units=imperial
    Processing Record 97 | Cotonou 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cotonou,bj&units=imperial
    Processing Record 98 | Iranshahr 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=iranshahr,ir&units=imperial
    City of Airai in PW not found
    Processing Record 100 | Kotido 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kotido,ug&units=imperial
    Processing Record 101 | Koumac 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=koumac,nc&units=imperial
    Processing Record 102 | Kelvington 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kelvington,ca&units=imperial
    Processing Record 103 | Kurmanayevka 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kurmanayevka,ru&units=imperial
    Processing Record 104 | Kaseda 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kaseda,jp&units=imperial
    Processing Record 105 | Blankenberge 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=blankenberge,be&units=imperial
    Processing Record 106 | Sakaiminato 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sakaiminato,jp&units=imperial
    Processing Record 107 | Comodoro Rivadavia 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=comodoro rivadavia,ar&units=imperial
    Processing Record 108 | Phatthalung 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=phatthalung,th&units=imperial
    Processing Record 109 | Algiers 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=algiers,dz&units=imperial
    Processing Record 110 | Luchegorsk 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=luchegorsk,ru&units=imperial
    Processing Record 111 | Yanam 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=yanam,in&units=imperial
    Processing Record 112 | Ormond Beach 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ormond beach,us&units=imperial
    Processing Record 113 | Hamilton 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hamilton,bm&units=imperial
    Processing Record 114 | Moree 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=moree,au&units=imperial
    Processing Record 115 | San Miguel 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=san miguel,pa&units=imperial
    Processing Record 116 | Tartagal 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tartagal,ar&units=imperial
    Processing Record 117 | Ayyampettai 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ayyampettai,in&units=imperial
    Processing Record 118 | Phibun Mangsahan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=phibun mangsahan,th&units=imperial
    Processing Record 119 | Emerald 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=emerald,au&units=imperial
    Processing Record 120 | Vao 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vao,nc&units=imperial
    Processing Record 121 | Sobolevo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sobolevo,ru&units=imperial
    Processing Record 122 | Hithadhoo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hithadhoo,mv&units=imperial
    Processing Record 123 | Chokurdakh 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=chokurdakh,ru&units=imperial
    Processing Record 124 | Puri 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=puri,in&units=imperial
    Processing Record 125 | Labuan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=labuan,my&units=imperial
    Processing Record 126 | Salalah 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=salalah,om&units=imperial
    City of Barentsburg in SJ not found
    Processing Record 128 | Osakarovka 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=osakarovka,kz&units=imperial
    Processing Record 129 | San Jose 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=san jose,gt&units=imperial
    Processing Record 130 | Kabanga 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kabanga,tz&units=imperial
    Processing Record 131 | Ziway 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ziway,et&units=imperial
    Processing Record 132 | Tual 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tual,id&units=imperial
    Processing Record 133 | Atuona 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=atuona,pf&units=imperial
    City of San Quintin in MX not found
    Processing Record 135 | Sooke 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sooke,ca&units=imperial
    Processing Record 136 | Broome 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=broome,au&units=imperial
    Processing Record 137 | Sioux Lookout 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sioux lookout,ca&units=imperial
    Processing Record 138 | Santa Maria 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=santa maria,cv&units=imperial
    Processing Record 139 | Sorong 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sorong,id&units=imperial
    Processing Record 140 | Kisanga 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kisanga,tz&units=imperial
    Processing Record 141 | Promyshlennyy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=promyshlennyy,ru&units=imperial
    Processing Record 142 | Haikou 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=haikou,cn&units=imperial
    Processing Record 143 | Coolum Beach 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=coolum beach,au&units=imperial
    Processing Record 144 | Acapulco 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=acapulco,mx&units=imperial
    Processing Record 145 | Butaritari 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=butaritari,ki&units=imperial
    City of Cheuskiny in RU not found
    Processing Record 147 | Bonavista 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bonavista,ca&units=imperial
    Processing Record 148 | Imbituba 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=imbituba,br&units=imperial
    Processing Record 149 | Katsuura 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=katsuura,jp&units=imperial
    Processing Record 150 | Half Moon Bay 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=half moon bay,us&units=imperial
    Processing Record 151 | Huarmey 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=huarmey,pe&units=imperial
    Processing Record 152 | Atambua 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=atambua,id&units=imperial
    Processing Record 153 | Werda 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=werda,bw&units=imperial
    Processing Record 154 | Mokokchung 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mokokchung,in&units=imperial
    Processing Record 155 | Hartford 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hartford,us&units=imperial
    Processing Record 156 | Yellowknife 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=yellowknife,ca&units=imperial
    Processing Record 157 | Jamestown 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=jamestown,sh&units=imperial
    Processing Record 158 | Punta Arenas 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=punta arenas,cl&units=imperial
    Processing Record 159 | Kodiak 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kodiak,us&units=imperial
    Processing Record 160 | Marienburg 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=marienburg,sr&units=imperial
    Processing Record 161 | Slave Lake 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=slave lake,ca&units=imperial
    Processing Record 162 | Arona 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=arona,es&units=imperial
    Processing Record 163 | Sibolga 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sibolga,id&units=imperial
    Processing Record 164 | Mandalgovi 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mandalgovi,mn&units=imperial
    Processing Record 165 | Puerto Escondido 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=puerto escondido,mx&units=imperial
    Processing Record 166 | Saint-Joseph 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=saint-joseph,re&units=imperial
    Processing Record 167 | Hilo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hilo,us&units=imperial
    Processing Record 168 | Nampa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=nampa,us&units=imperial
    City of Mahadday Weyne in SO not found
    Processing Record 170 | Shimoda 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=shimoda,jp&units=imperial
    Processing Record 171 | Paulo Ramos 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=paulo ramos,br&units=imperial
    Processing Record 172 | Plouzane 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=plouzane,fr&units=imperial
    Processing Record 173 | Necochea 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=necochea,ar&units=imperial
    City of Halalo in WF not found
    Processing Record 175 | Alugan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=alugan,ph&units=imperial
    Processing Record 176 | Mnogovershinnyy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mnogovershinnyy,ru&units=imperial
    Processing Record 177 | Wattegama 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=wattegama,lk&units=imperial
    Processing Record 178 | Ketchikan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ketchikan,us&units=imperial
    Processing Record 179 | Iquique 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=iquique,cl&units=imperial
    Processing Record 180 | Burgeo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=burgeo,ca&units=imperial
    Processing Record 181 | Isangel 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=isangel,vu&units=imperial
    Processing Record 182 | Cherskiy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cherskiy,ru&units=imperial
    Processing Record 183 | Erzin 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=erzin,ru&units=imperial
    Processing Record 184 | Merauke 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=merauke,id&units=imperial
    Processing Record 185 | San Fernando 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=san fernando,mx&units=imperial
    Processing Record 186 | Khandyga 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=khandyga,ru&units=imperial
    Processing Record 187 | Bambanglipuro 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bambanglipuro,id&units=imperial
    City of Vaitupu in WF not found
    Processing Record 189 | Tilichiki 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tilichiki,ru&units=imperial
    Processing Record 190 | Mahibadhoo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mahibadhoo,mv&units=imperial
    Processing Record 191 | Gejiu 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=gejiu,cn&units=imperial
    Processing Record 192 | Chalus 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=chalus,ir&units=imperial
    Processing Record 193 | Grand Gaube 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=grand gaube,mu&units=imperial
    City of Isabela in US not found
    Processing Record 195 | Iqaluit 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=iqaluit,ca&units=imperial
    Processing Record 196 | Viedma 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=viedma,ar&units=imperial
    Processing Record 197 | Menongue 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=menongue,ao&units=imperial
    Processing Record 198 | Sturgeon Bay 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sturgeon bay,us&units=imperial
    Processing Record 199 | Popova 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=popova,ru&units=imperial
    Processing Record 200 | Ovar 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ovar,pt&units=imperial
    Processing Record 201 | Kvitok 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kvitok,ru&units=imperial
    Processing Record 202 | Tarata 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tarata,pe&units=imperial
    Processing Record 203 | Talnakh 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=talnakh,ru&units=imperial
    Processing Record 204 | Celestun 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=celestun,mx&units=imperial
    Processing Record 205 | Qaanaaq 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=qaanaaq,gl&units=imperial
    Processing Record 206 | Victoria 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=victoria,sc&units=imperial
    Processing Record 207 | Taoudenni 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=taoudenni,ml&units=imperial
    Processing Record 208 | Acari 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=acari,pe&units=imperial
    Processing Record 209 | Te Anau 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=te anau,nz&units=imperial
    City of Nizhneyansk in RU not found
    Processing Record 211 | Marawi 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=marawi,sd&units=imperial
    Processing Record 212 | Kashan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kashan,ir&units=imperial
    Processing Record 213 | Pisco 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pisco,pe&units=imperial
    Processing Record 214 | Port Blair 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=port blair,in&units=imperial
    Processing Record 215 | Karratha 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=karratha,au&units=imperial
    Processing Record 216 | Antofagasta 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=antofagasta,cl&units=imperial
    Processing Record 217 | Bartica 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bartica,gy&units=imperial
    Processing Record 218 | Kenai 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kenai,us&units=imperial
    Processing Record 219 | Gagret 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=gagret,in&units=imperial
    Processing Record 220 | Turbat 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=turbat,pk&units=imperial
    Processing Record 221 | Novopavlovka 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=novopavlovka,ru&units=imperial
    Processing Record 222 | Kahului 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kahului,us&units=imperial
    City of Mahaicony in GY not found
    Processing Record 224 | Alyangula 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=alyangula,au&units=imperial
    Processing Record 225 | Adelaide 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=adelaide,au&units=imperial
    Processing Record 226 | Hovd 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hovd,mn&units=imperial
    Processing Record 227 | The Valley 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=the valley,ai&units=imperial
    Processing Record 228 | Ternate 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ternate,id&units=imperial
    City of Mys Shmidta in RU not found
    Processing Record 230 | Kuala Terengganu 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kuala terengganu,my&units=imperial
    Processing Record 231 | Mareeba 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mareeba,au&units=imperial
    City of Kismayo in SO not found
    Processing Record 233 | Honiara 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=honiara,sb&units=imperial
    Processing Record 234 | Kushima 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kushima,jp&units=imperial
    City of Utiroa in KI not found
    Processing Record 236 | Pimenta Bueno 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pimenta bueno,br&units=imperial
    City of Bokspits in BW not found
    Processing Record 238 | Thompson 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=thompson,ca&units=imperial
    Processing Record 239 | Bucerias 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bucerias,mx&units=imperial
    City of Tongsa in BT not found
    Processing Record 241 | Kualakapuas 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kualakapuas,id&units=imperial
    Processing Record 242 | Giyani 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=giyani,za&units=imperial
    Processing Record 243 | Pevek 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pevek,ru&units=imperial
    Processing Record 244 | Bowen 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bowen,au&units=imperial
    Processing Record 245 | Clyde River 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=clyde river,ca&units=imperial
    City of Iskateley in RU not found
    Processing Record 247 | Oriximina 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=oriximina,br&units=imperial
    Processing Record 248 | Banda Aceh 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=banda aceh,id&units=imperial
    Processing Record 249 | Vestmannaeyjar 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vestmannaeyjar,is&units=imperial
    Processing Record 250 | Namatanai 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=namatanai,pg&units=imperial
    Processing Record 251 | Wahpeton 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=wahpeton,us&units=imperial
    Processing Record 252 | Kapuskasing 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kapuskasing,ca&units=imperial
    Processing Record 253 | Kajaani 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kajaani,fi&units=imperial
    Processing Record 254 | Podzvizd 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=podzvizd,ba&units=imperial
    Processing Record 255 | Mocuba 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mocuba,mz&units=imperial
    Processing Record 256 | Snezhnogorsk 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=snezhnogorsk,ru&units=imperial
    Processing Record 257 | Panjab 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=panjab,af&units=imperial
    Processing Record 258 | Biak 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=biak,id&units=imperial
    Processing Record 259 | Bethel 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bethel,us&units=imperial
    City of Dzhusaly in KZ not found
    Processing Record 261 | Yinchuan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=yinchuan,cn&units=imperial
    Processing Record 262 | Pozo Colorado 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pozo colorado,py&units=imperial
    Processing Record 263 | Plettenberg Bay 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=plettenberg bay,za&units=imperial
    City of Galgani in SD not found
    Processing Record 265 | Ozinki 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ozinki,ru&units=imperial
    City of Dolbeau in CA not found
    Processing Record 267 | Basco 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=basco,ph&units=imperial
    Processing Record 268 | Avarua 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=avarua,ck&units=imperial
    Processing Record 269 | Jacareacanga 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=jacareacanga,br&units=imperial
    Processing Record 270 | Placido De Castro 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=placido de castro,br&units=imperial
    Processing Record 271 | Cairns 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cairns,au&units=imperial
    Processing Record 272 | Cerrito 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cerrito,py&units=imperial
    Processing Record 273 | Yulara 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=yulara,au&units=imperial
    Processing Record 274 | College 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=college,us&units=imperial
    Processing Record 275 | Provideniya 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=provideniya,ru&units=imperial
    Processing Record 276 | Beringovskiy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=beringovskiy,ru&units=imperial
    Processing Record 277 | Port Hedland 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=port hedland,au&units=imperial
    Processing Record 278 | Faanui 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=faanui,pf&units=imperial
    Processing Record 279 | Sitka 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sitka,us&units=imperial
    Processing Record 280 | Castro 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=castro,cl&units=imperial
    Processing Record 281 | Concarneau 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=concarneau,fr&units=imperial
    Processing Record 282 | Srednekolymsk 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=srednekolymsk,ru&units=imperial
    Processing Record 283 | Ugoofaaru 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ugoofaaru,mv&units=imperial
    Processing Record 284 | Baykit 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=baykit,ru&units=imperial
    Processing Record 285 | Neyshabur 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=neyshabur,ir&units=imperial
    City of Lasa in CN not found
    Processing Record 287 | Naze 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=naze,jp&units=imperial
    Processing Record 288 | Cidreira 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cidreira,br&units=imperial
    City of Fort Saint John in CA not found
    Processing Record 290 | Yamethin 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=yamethin,mm&units=imperial
    Processing Record 291 | Kununurra 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kununurra,au&units=imperial
    Processing Record 292 | Marsa Matruh 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=marsa matruh,eg&units=imperial
    Processing Record 293 | Shikohabad 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=shikohabad,in&units=imperial
    Processing Record 294 | Gushikawa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=gushikawa,jp&units=imperial
    Processing Record 295 | Port Lincoln 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=port lincoln,au&units=imperial
    Processing Record 296 | Shaygino 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=shaygino,ru&units=imperial
    Processing Record 297 | Waddan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=waddan,ly&units=imperial
    Processing Record 298 | Aitape 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=aitape,pg&units=imperial
    Processing Record 299 | Along 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=along,in&units=imperial
    Processing Record 300 | Sola 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sola,vu&units=imperial
    Processing Record 301 | Tura 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tura,ru&units=imperial
    Processing Record 302 | Vinces 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vinces,ec&units=imperial
    Processing Record 303 | Gladstone 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=gladstone,au&units=imperial
    Processing Record 304 | Hambantota 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hambantota,lk&units=imperial
    Processing Record 305 | Usinsk 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=usinsk,ru&units=imperial
    Processing Record 306 | Kedrovyy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kedrovyy,ru&units=imperial
    City of Hunza in PK not found
    Processing Record 308 | Baracoa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=baracoa,cu&units=imperial
    Processing Record 309 | Mayumba 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mayumba,ga&units=imperial
    Processing Record 310 | Ambovombe 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ambovombe,mg&units=imperial
    Processing Record 311 | Tapes 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tapes,br&units=imperial
    Processing Record 312 | Ambilobe 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ambilobe,mg&units=imperial
    City of Esna in EG not found
    Processing Record 314 | Aksarka 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=aksarka,ru&units=imperial
    City of Sentyabrskiy in RU not found
    City of Laguna in BR not found
    Processing Record 317 | Severo-Kurilsk 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=severo-kurilsk,ru&units=imperial
    Processing Record 318 | Arraial Do Cabo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=arraial do cabo,br&units=imperial
    Processing Record 319 | Soyo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=soyo,ao&units=imperial
    Processing Record 320 | Flin Flon 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=flin flon,ca&units=imperial
    Processing Record 321 | Ushuaia 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ushuaia,ar&units=imperial
    Processing Record 322 | Tuatapere 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tuatapere,nz&units=imperial
    Processing Record 323 | Akhtubinsk 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=akhtubinsk,ru&units=imperial
    Processing Record 324 | Channel-Port Aux Basques 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=channel-port aux basques,ca&units=imperial
    Processing Record 325 | Ventersburg 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ventersburg,za&units=imperial
    Processing Record 326 | Gamba 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=gamba,ga&units=imperial
    City of Parras in MX not found
    Processing Record 328 | Amapa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=amapa,br&units=imperial
    City of Codrington in AG not found
    Processing Record 330 | Kenora 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kenora,ca&units=imperial
    City of Kegayli in UZ not found
    Processing Record 332 | Lagunillas 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=lagunillas,ve&units=imperial
    City of Bengkulu in ID not found
    Processing Record 334 | Tautira 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tautira,pf&units=imperial
    Processing Record 335 | Mahabad 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mahabad,ir&units=imperial
    Processing Record 336 | Port Macquarie 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=port macquarie,au&units=imperial
    Processing Record 337 | Praya 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=praya,id&units=imperial
    Processing Record 338 | Ucluelet 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ucluelet,ca&units=imperial
    Processing Record 339 | Poum 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=poum,nc&units=imperial
    Processing Record 340 | Albany 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=albany,au&units=imperial
    Processing Record 341 | Centralia 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=centralia,us&units=imperial
    Processing Record 342 | Sao Joao Da Barra 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sao joao da barra,br&units=imperial
    Processing Record 343 | Pangnirtung 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pangnirtung,ca&units=imperial
    Processing Record 344 | Cape Town 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cape town,za&units=imperial
    Processing Record 345 | Codajas 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=codajas,br&units=imperial
    City of Attawapiskat in CA not found
    Processing Record 347 | Lebu 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=lebu,cl&units=imperial
    Processing Record 348 | Cedar City 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cedar city,us&units=imperial
    Processing Record 349 | Vidim 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vidim,ru&units=imperial
    Processing Record 350 | Dikson 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=dikson,ru&units=imperial
    Processing Record 351 | Sestri Levante 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sestri levante,it&units=imperial
    Processing Record 352 | Melfi 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=melfi,td&units=imperial
    Processing Record 353 | Xuddur 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=xuddur,so&units=imperial
    Processing Record 354 | Dunedin 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=dunedin,nz&units=imperial
    Processing Record 355 | Port Elizabeth 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=port elizabeth,za&units=imperial
    Processing Record 356 | Graaff-Reinet 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=graaff-reinet,za&units=imperial
    Processing Record 357 | Cayenne 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cayenne,gf&units=imperial
    Processing Record 358 | Mar Del Plata 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mar del plata,ar&units=imperial
    City of Envira in BR not found
    Processing Record 360 | Takoradi 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=takoradi,gh&units=imperial
    Processing Record 361 | Agirish 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=agirish,ru&units=imperial
    Processing Record 362 | Shakawe 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=shakawe,bw&units=imperial
    City of Makung in TW not found
    Processing Record 364 | Staryy Nadym 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=staryy nadym,ru&units=imperial
    Processing Record 365 | Lagoa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=lagoa,pt&units=imperial
    Processing Record 366 | El Dorado 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=el dorado,us&units=imperial
    City of Jomalig in PH not found
    Processing Record 368 | Oussouye 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=oussouye,sn&units=imperial
    Processing Record 369 | Kayerkan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kayerkan,ru&units=imperial
    Processing Record 370 | Pingliang 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pingliang,cn&units=imperial
    Processing Record 371 | Kapaa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kapaa,us&units=imperial
    Processing Record 372 | Iralaya 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=iralaya,hn&units=imperial
    Processing Record 373 | Zapolyarnyy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=zapolyarnyy,ru&units=imperial
    City of Mafinga in TZ not found
    Processing Record 375 | Hobyo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hobyo,so&units=imperial
    Processing Record 376 | Byron Bay 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=byron bay,au&units=imperial
    Processing Record 377 | Chubbuck 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=chubbuck,us&units=imperial
    Processing Record 378 | Manzhouli 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=manzhouli,cn&units=imperial
    Processing Record 379 | Murakami 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=murakami,jp&units=imperial
    Processing Record 380 | Mazamari 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mazamari,pe&units=imperial
    City of Goderich in SL not found
    Processing Record 382 | Umm Kaddadah 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=umm kaddadah,sd&units=imperial
    Processing Record 383 | Muzhi 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=muzhi,ru&units=imperial
    Processing Record 384 | Vrangel 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vrangel,ru&units=imperial
    Processing Record 385 | Hami 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hami,cn&units=imperial
    Processing Record 386 | Beloha 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=beloha,mg&units=imperial
    Processing Record 387 | Nemuro 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=nemuro,jp&units=imperial
    Processing Record 388 | Porto Belo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=porto belo,br&units=imperial
    Processing Record 389 | Bardiyah 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bardiyah,ly&units=imperial
    Processing Record 390 | Santa Rosa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=santa rosa,ar&units=imperial
    Processing Record 391 | Puerto Rico 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=puerto rico,co&units=imperial
    Processing Record 392 | Port Hardy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=port hardy,ca&units=imperial
    City of Grand River South East in MU not found
    Processing Record 394 | Touros 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=touros,br&units=imperial
    Processing Record 395 | Tasiilaq 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tasiilaq,gl&units=imperial
    Processing Record 396 | Barcelos 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=barcelos,br&units=imperial
    Processing Record 397 | Englehart 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=englehart,ca&units=imperial
    Processing Record 398 | Bakel 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bakel,sn&units=imperial
    City of Amderma in RU not found
    City of Sim in RU not found
    Processing Record 401 | Road Town 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=road town,vg&units=imperial
    Processing Record 402 | Flinders 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=flinders,au&units=imperial
    Processing Record 403 | Bukama 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bukama,cd&units=imperial
    City of Belushya Guba in RU not found
    Processing Record 405 | Nuuk 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=nuuk,gl&units=imperial
    City of Khormuj in IR not found
    City of Bose in CN not found
    Processing Record 408 | Bongandanga 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bongandanga,cd&units=imperial
    Processing Record 409 | Khatanga 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=khatanga,ru&units=imperial
    Processing Record 410 | Udachnyy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=udachnyy,ru&units=imperial
    Processing Record 411 | Bushehr 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bushehr,ir&units=imperial
    Processing Record 412 | Itoman 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=itoman,jp&units=imperial
    Processing Record 413 | Bondowoso 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bondowoso,id&units=imperial
    Processing Record 414 | Azangaro 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=azangaro,pe&units=imperial
    Processing Record 415 | Alihe 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=alihe,cn&units=imperial
    Processing Record 416 | Otane 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=otane,nz&units=imperial
    Processing Record 417 | Samfya 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=samfya,zm&units=imperial
    Processing Record 418 | Wuan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=wuan,cn&units=imperial
    Processing Record 419 | Vila Franca Do Campo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vila franca do campo,pt&units=imperial
    Processing Record 420 | Rastu 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=rastu,ro&units=imperial
    Processing Record 421 | Upernavik 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=upernavik,gl&units=imperial
    Processing Record 422 | Mount Isa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mount isa,au&units=imperial
    Processing Record 423 | Vardo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vardo,no&units=imperial
    Processing Record 424 | Saint Augustine 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=saint augustine,us&units=imperial
    City of Phan Rang in VN not found
    Processing Record 426 | Santa Maria Da Boa Vista 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=santa maria da boa vista,br&units=imperial
    City of Palabuhanratu in ID not found
    Processing Record 428 | Beidao 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=beidao,cn&units=imperial
    Processing Record 429 | Vaini 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vaini,to&units=imperial
    Processing Record 430 | Skelleftea 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=skelleftea,se&units=imperial
    Processing Record 431 | Saint-Philippe 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=saint-philippe,re&units=imperial
    Processing Record 432 | Lexington 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=lexington,us&units=imperial
    Processing Record 433 | Utinga 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=utinga,br&units=imperial
    Processing Record 434 | Yar-Sale 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=yar-sale,ru&units=imperial
    Processing Record 435 | Edd 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=edd,er&units=imperial
    City of Olafsvik in IS not found
    Processing Record 437 | Ribeira Grande 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ribeira grande,pt&units=imperial
    Processing Record 438 | Hede 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hede,cn&units=imperial
    Processing Record 439 | Ixtapa 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ixtapa,mx&units=imperial
    Processing Record 440 | Margate 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=margate,za&units=imperial
    Processing Record 441 | Mao 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mao,td&units=imperial
    Processing Record 442 | Los Llanos De Aridane 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=los llanos de aridane,es&units=imperial
    Processing Record 443 | Camacha 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=camacha,pt&units=imperial
    City of Taolanaro in MG not found
    City of Severnyy in RU not found
    Processing Record 446 | Rincon 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=rincon,us&units=imperial
    Processing Record 447 | Ust-Tsilma 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ust-tsilma,ru&units=imperial
    Processing Record 448 | Changping 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=changping,cn&units=imperial
    Processing Record 449 | Bathsheba 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bathsheba,bb&units=imperial
    Processing Record 450 | Constitucion 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=constitucion,mx&units=imperial
    Processing Record 451 | Ancud 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ancud,cl&units=imperial
    Processing Record 452 | Sinnamary 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sinnamary,gf&units=imperial
    Processing Record 453 | Dhidhdhoo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=dhidhdhoo,mv&units=imperial
    Processing Record 454 | Carutapera 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=carutapera,br&units=imperial
    Processing Record 455 | San Joaquin 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=san joaquin,bo&units=imperial
    Processing Record 456 | Praia Da Vitoria 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=praia da vitoria,pt&units=imperial
    Processing Record 457 | Kaeo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kaeo,nz&units=imperial
    Processing Record 458 | Carnarvon 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=carnarvon,au&units=imperial
    City of Mullaitivu in LK not found
    Processing Record 460 | Ati 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ati,td&units=imperial
    Processing Record 461 | Preobrazheniye 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=preobrazheniye,ru&units=imperial
    Processing Record 462 | Richards Bay 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=richards bay,za&units=imperial
    Processing Record 463 | Aksu 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=aksu,cn&units=imperial
    Processing Record 464 | Vostok 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vostok,ru&units=imperial
    Processing Record 465 | Berbera 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=berbera,so&units=imperial
    Processing Record 466 | Wladyslawowo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=wladyslawowo,pl&units=imperial
    Processing Record 467 | Chiang Rai 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=chiang rai,th&units=imperial
    Processing Record 468 | Dumas 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=dumas,us&units=imperial
    Processing Record 469 | Garowe 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=garowe,so&units=imperial
    Processing Record 470 | Vila Velha 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vila velha,br&units=imperial
    Processing Record 471 | Lompoc 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=lompoc,us&units=imperial
    Processing Record 472 | Hobart 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hobart,au&units=imperial
    Processing Record 473 | Dahanu 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=dahanu,in&units=imperial
    Processing Record 474 | Awjilah 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=awjilah,ly&units=imperial
    Processing Record 475 | Gisborne 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=gisborne,nz&units=imperial
    Processing Record 476 | Cerkezkoy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=cerkezkoy,tr&units=imperial
    Processing Record 477 | High Level 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=high level,ca&units=imperial
    Processing Record 478 | Vernon 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vernon,fr&units=imperial
    Processing Record 479 | Grimshaw 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=grimshaw,ca&units=imperial
    Processing Record 480 | Sangar 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sangar,ru&units=imperial
    Processing Record 481 | Igarka 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=igarka,ru&units=imperial
    City of Illoqqortoormiut in GL not found
    Processing Record 483 | East London 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=east london,za&units=imperial
    Processing Record 484 | Georgetown 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=georgetown,sh&units=imperial
    Processing Record 485 | Hastings 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hastings,us&units=imperial
    Processing Record 486 | Koygorodok 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=koygorodok,ru&units=imperial
    Processing Record 487 | Fortuna 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=fortuna,us&units=imperial
    Processing Record 488 | Westport 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=westport,ie&units=imperial
    Processing Record 489 | Kudahuvadhoo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kudahuvadhoo,mv&units=imperial
    Processing Record 490 | Kokstad 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kokstad,za&units=imperial
    Processing Record 491 | Parrsboro 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=parrsboro,ca&units=imperial
    Processing Record 492 | Hofn 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hofn,is&units=imperial
    Processing Record 493 | Zabol 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=zabol,ir&units=imperial
    Processing Record 494 | Pokhara 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pokhara,np&units=imperial
    City of Lata in SB not found
    Processing Record 496 | Koutsouras 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=koutsouras,gr&units=imperial
    Processing Record 497 | Batemans Bay 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=batemans bay,au&units=imperial
    Processing Record 498 | Inhambane 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=inhambane,mz&units=imperial
    Processing Record 499 | Pasighat 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pasighat,in&units=imperial
    Processing Record 500 | Leningradskiy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=leningradskiy,ru&units=imperial
    Processing Record 501 | Gillette 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=gillette,us&units=imperial
    Processing Record 502 | Beruwala 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=beruwala,lk&units=imperial
    Processing Record 503 | Talcahuano 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=talcahuano,cl&units=imperial
    Processing Record 504 | Ahipara 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ahipara,nz&units=imperial
    Processing Record 505 | Altay 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=altay,cn&units=imperial
    City of Tsihombe in MG not found
    Processing Record 507 | Dukat 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=dukat,ru&units=imperial
    Processing Record 508 | Santa Fe 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=santa fe,cu&units=imperial
    Processing Record 509 | Nhulunbuy 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=nhulunbuy,au&units=imperial
    Processing Record 510 | Mayo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mayo,ca&units=imperial
    City of Gat in LY not found
    Processing Record 512 | Ilulissat 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ilulissat,gl&units=imperial
    Processing Record 513 | Namibe 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=namibe,ao&units=imperial
    Processing Record 514 | Frankfort 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=frankfort,za&units=imperial
    Processing Record 515 | Vada 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=vada,in&units=imperial
    Processing Record 516 | Aklavik 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=aklavik,ca&units=imperial
    City of Mahon in ES not found
    Processing Record 518 | Alice Springs 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=alice springs,au&units=imperial
    Processing Record 519 | Sergach 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sergach,ru&units=imperial
    Processing Record 520 | Bekwai 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bekwai,gh&units=imperial
    Processing Record 521 | Jalu 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=jalu,ly&units=imperial
    City of Samusu in WS not found
    Processing Record 523 | Belaya Gora 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=belaya gora,ru&units=imperial
    Processing Record 524 | Felanitx 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=felanitx,es&units=imperial
    Processing Record 525 | Zyryanka 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=zyryanka,ru&units=imperial
    Processing Record 526 | Luau 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=luau,ao&units=imperial
    Processing Record 527 | Ursulo Galvan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ursulo galvan,mx&units=imperial
    Processing Record 528 | Nantucket 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=nantucket,us&units=imperial
    Processing Record 529 | Rio Grande 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=rio grande,br&units=imperial
    Processing Record 530 | Bud 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bud,no&units=imperial
    City of Eskasem in AF not found
    Processing Record 532 | Klaksvik 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=klaksvik,fo&units=imperial
    Processing Record 533 | Muros 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=muros,es&units=imperial
    Processing Record 534 | Okha 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=okha,ru&units=imperial
    City of Japura in BR not found
    Processing Record 536 | Sumbe 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sumbe,ao&units=imperial
    Processing Record 537 | Hasaki 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=hasaki,jp&units=imperial
    Processing Record 538 | Sadovoye 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sadovoye,ru&units=imperial
    City of Hvammstangi in IS not found
    Processing Record 540 | Devils Lake 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=devils lake,us&units=imperial
    Processing Record 541 | Puerto Ayora 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=puerto ayora,ec&units=imperial
    Processing Record 542 | Shahr-E Kord 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=shahr-e kord,ir&units=imperial
    Processing Record 543 | Caravelas 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=caravelas,br&units=imperial
    Processing Record 544 | Porto Novo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=porto novo,cv&units=imperial
    Processing Record 545 | Kawalu 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kawalu,id&units=imperial
    Processing Record 546 | Machilipatnam 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=machilipatnam,in&units=imperial
    Processing Record 547 | Biloela 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=biloela,au&units=imperial
    Processing Record 548 | Pacific Grove 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=pacific grove,us&units=imperial
    Processing Record 549 | Merrill 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=merrill,us&units=imperial
    Processing Record 550 | Guerrero Negro 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=guerrero negro,mx&units=imperial
    Processing Record 551 | Saint George 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=saint george,bm&units=imperial
    Processing Record 552 | Chifeng 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=chifeng,cn&units=imperial
    Processing Record 553 | Saint-Pierre 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=saint-pierre,pm&units=imperial
    Processing Record 554 | Sahbuz 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sahbuz,az&units=imperial
    Processing Record 555 | Narsaq 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=narsaq,gl&units=imperial
    City of Maarianhamina in FI not found
    Processing Record 557 | Palembang 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=palembang,id&units=imperial
    Processing Record 558 | Kamaishi 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kamaishi,jp&units=imperial
    City of Geylegphug in BT not found
    Processing Record 560 | Mackenzie 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mackenzie,ca&units=imperial
    Processing Record 561 | Jimenez 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=jimenez,mx&units=imperial
    Processing Record 562 | Kysyl-Syr 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kysyl-syr,ru&units=imperial
    Processing Record 563 | Wanaka 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=wanaka,nz&units=imperial
    Processing Record 564 | Barra Patuca 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=barra patuca,hn&units=imperial
    Processing Record 565 | Marystown 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=marystown,ca&units=imperial
    Processing Record 566 | Banjar 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=banjar,id&units=imperial
    Processing Record 567 | Sur 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sur,om&units=imperial
    Processing Record 568 | Coihaique 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=coihaique,cl&units=imperial
    Processing Record 569 | Lima 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=lima,pe&units=imperial
    Processing Record 570 | Westlock 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=westlock,ca&units=imperial
    Processing Record 571 | Tiksi 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=tiksi,ru&units=imperial
    Processing Record 572 | Havoysund 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=havoysund,no&units=imperial
    Processing Record 573 | Sri Aman 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sri aman,my&units=imperial
    Processing Record 574 | Ngunguru 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ngunguru,nz&units=imperial
    Processing Record 575 | Mabaruma 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mabaruma,gy&units=imperial
    Processing Record 576 | Lorengau 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=lorengau,pg&units=imperial
    Processing Record 577 | Velka Nad Velickou 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=velka nad velickou,cz&units=imperial
    Processing Record 578 | Ponta Do Sol 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=ponta do sol,pt&units=imperial
    Processing Record 579 | Bend 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bend,us&units=imperial
    Processing Record 580 | Bredasdorp 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bredasdorp,za&units=imperial
    Processing Record 581 | Andenes 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=andenes,no&units=imperial
    Processing Record 582 | Sisophon 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sisophon,kh&units=imperial
    Processing Record 583 | Saint-Augustin 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=saint-augustin,ca&units=imperial
    Processing Record 584 | Almenara 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=almenara,br&units=imperial
    City of Jiddah in SA not found
    Processing Record 586 | Warrnambool 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=warrnambool,au&units=imperial
    Processing Record 587 | Polovinnoye 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=polovinnoye,ru&units=imperial
    Processing Record 588 | Mehamn 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mehamn,no&units=imperial
    Processing Record 589 | Rikitea 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=rikitea,pf&units=imperial
    Processing Record 590 | Darhan 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=darhan,mn&units=imperial
    Processing Record 591 | Bluff 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bluff,nz&units=imperial
    Processing Record 592 | Curuca 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=curuca,br&units=imperial
    City of Artyk in RU not found
    Processing Record 594 | Paamiut 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=paamiut,gl&units=imperial
    Processing Record 595 | Bagua Grande 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bagua grande,pe&units=imperial
    Processing Record 596 | Geraldton 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=geraldton,au&units=imperial
    Processing Record 597 | Sawtell 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=sawtell,au&units=imperial
    City of Wampusirpi in HN not found
    Processing Record 599 | Kanniyakumari 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=kanniyakumari,in&units=imperial
    Processing Record 600 | Mariental 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mariental,na&units=imperial
    Processing Record 601 | Panama City 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=panama city,us&units=imperial
    Processing Record 602 | Norman Wells 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=norman wells,ca&units=imperial
    Processing Record 603 | Mogadouro 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=mogadouro,pt&units=imperial
    Processing Record 604 | Bashtanka 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=bashtanka,ua&units=imperial
    Processing Record 605 | Funadhoo 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=funadhoo,mv&units=imperial
    City of Mirina in GR not found
    City of Burica in PA not found
    Processing Record 608 | Londoko 
    http://api.openweathermap.org/data/2.5/weather?apikey=3b6fc998272abf055734591058b4c655&q=londoko,ru&units=imperial
    
    ----------------------------
     Data Retrieval complete
    ----------------------------
    


```python
weather_info.to_csv('weather_info.csv')
weather_info.count()
```




    City           539
    Cloudiness     539
    Country        539
    Date           539
    Humidity       539
    Lat            539
    Lng            539
    Temperature    539
    Wind Speed     539
    dtype: int64




```python
weather_info.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Date</th>
      <th>Humidity</th>
      <th>Lat</th>
      <th>Lng</th>
      <th>Temperature</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Roald</td>
      <td>36</td>
      <td>NO</td>
      <td>1514348400</td>
      <td>74</td>
      <td>62.58</td>
      <td>6.12</td>
      <td>35.60</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Benito Juarez</td>
      <td>24</td>
      <td>MX</td>
      <td>1514350711</td>
      <td>67</td>
      <td>28.64</td>
      <td>-111.57</td>
      <td>55.59</td>
      <td>6.29</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dioknisi</td>
      <td>0</td>
      <td>GE</td>
      <td>1514349000</td>
      <td>57</td>
      <td>41.63</td>
      <td>42.39</td>
      <td>53.60</td>
      <td>28.86</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Cururupu</td>
      <td>48</td>
      <td>BR</td>
      <td>1514350712</td>
      <td>92</td>
      <td>-1.82</td>
      <td>-44.87</td>
      <td>77.55</td>
      <td>3.94</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Fort Nelson</td>
      <td>90</td>
      <td>CA</td>
      <td>1514347200</td>
      <td>76</td>
      <td>58.81</td>
      <td>-122.69</td>
      <td>-11.21</td>
      <td>3.36</td>
    </tr>
  </tbody>
</table>
</div>



### Latitude vs Temperature Plot


```python
df=weather_info.set_index('Lat')
def plot_data(ylabels, uylabels, values):
    sns.set("notebook",font_scale=1.2)
    fig, ax=plt.subplots(figsize=(12,8), linewidth=0.1)

    ax.set_ylabel(ylabels + uylabels)
    ax.set_xlabel('Latitude')
    ax.set_title('City Latitude vs ' + ylabels + '(%s)' % (date.today().strftime('%m/%d/%Y')))
    ax.minorticks_on()
    ax.set_xlim(-90, 90)

    ax.grid(True,linestyle='dashed')
    ax.scatter(df.index, values, c='b', edgecolor='k')
    fig.tight_layout()
    plt.savefig('Latitude_vs_%s_(%s)' % (ylabels, date.today().strftime('%m-%d-%Y')))
    plt.show()

plot_data('Max Temperature ', '(F)', df['Temperature'])
```


![png](output_10_0.png)


#### Fig 1: Max Temperature increases from from approximately -58 degrees Latitude to 0 degrees Latitude, then take a sharp decrease between 0 and 80 degrees Latitude.

### Humidity vs Temperature Plot


```python
plot_data('Humidity Plot ', '(%)', df['Humidity'])
```


![png](output_13_0.png)


#### Fig 2: Humidity is relatively high between -40 and 80 degrees Latitude, but starts to decrease at about -50 degrees latitude.

### Cloudiness vs Temperature Plot


```python
plot_data('Cloudiness ', '(%)', df['Cloudiness'])
```


![png](output_16_0.png)


#### Fig 3: Level of cloudiness is evenly distributed at -49 to 80 degrees Latitude, but seems to falll significantly and eventually to zero at < -40 degrees Latitude.

### Wind Speed vs Temperature Plot


```python
plot_data('Wind Speed ', '(mph)', df['Wind Speed'])
```


![png](output_19_0.png)


#### Fig 4: LAtitude doesn't seem to have an effect on wind speed as wind speed trends low with a few outliers occuring between 30 and 60 degrees Latitude.
