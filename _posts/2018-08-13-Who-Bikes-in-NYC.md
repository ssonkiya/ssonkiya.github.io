The 6th largest bike share system in the world, CitiBike has fit into the fabric of this city seamlessly, and is now as quintessentially "New York" a sight as the classic yellow cab. I have been a long time user, even quitting MTA entirely at a point, so I was really looking forward to digging into the Citibike dataset and learning a bit more about geographical analysis.

One immediate thing that stood out to me was the imbalance between "customers," who have purchased a 3 or 7 day pass, and annual "subscribers." I would imagine that distinction falls heavily between tourists vs locals, and I was surprised that so few users were tourists, since this city sees approx 60 million visitors a year, while only 8.5 million people call it home. If you ask me, the **BEST** way to see this city is by bike, so I imagine that a lot of tourists are missing out on an amazing New York City experience, while Citibike is missing out on a lot of potential _customers_. 

![duration](/images/citibike/pie.png)

So, who are these customers, and which stations do they frequent? Perhaps the data can inform Citibike on how to appeal to this type of user.
 
### Popular Stations

Below are the most popular stations for **subscribers**;

<iframe src="https://public.tableau.com/profile/katie4698#!/vizhome/CitibikeStationsByUsers/subscribers?publish=yes"
width="645" height="955"></iframe>

The highlighted stations are the top ten. It's not surprising that central Manhattan is highly represented, especially NoMad, a very "up and coming" neighborhood popular amongst start ups. I would imagine that people are commuting to those stations. However, I was quite surprised at Grand Central being the top station, beating the runner up by about 50%. This tells me that a lot of these "subscribers" are actually people who commute in from the suburbs and then use Citibike to get around once they are here.

![duration](/images/citibike/topsubscriberstations.png)

When we look at top **customer stations**, the difference between where the two classes travel becomes quite apparent;

<iframe src="https://public.tableau.com/profile/katie4698#!/vizhome/CitibikeStationsByUsers/customers?publish=yes"
width="645" height="955"></iframe>

![duration](/images/citibike/topcustomerstations.png)

The top customer stations are also no surprise - They are concentrated heavily around 2 distinct "tourism" areas: Central Park (6 of the top 10 are ON the park) and downtown. Notably West & Chambers and Centre and Chambers; these two stations are located at World Trade Center & the Brooklyn Bridge respectively. Additionally, there was one station that seemed like an outlier until I looked it up on Google Maps. **12th ave & 40th street** is right near a sight-seeing cruise pier, the pier that houses the Intrepid Air & Space Museum, and it's right next to the Javits Convention Center, so it stands to reason that visitors would frequent that station.  

Hopefully these differences mean that I can build a reasonably good classification model to distinguish between trips taken by customers and trips taken by subscribers.

### The Data

The data is primarily "trip" data; one observation is one trip within the system. Each trip includes start station information (station id, name, latitude ,longitude) end station information, bike id, uesr type, and an optional birthyear and gender.  Approx only 5% of my target _customers_ gave gender, and only 17% gave birthyear. 

Station information (such as number of open docks, number of bikes, and total docks at a station) is provided separately in the form of a current snap shot of the system, so there is no historical data. 

### The Model

The first thing that I did was assign each station a neighborhood, since this sort of information may be useful in distinguishing customers from subscribers. I did this by pulling [geojson information](https://data.cityofnewyork.us/City-Government/Neighborhood-Tabulation-Areas/cpf4-rkhq) from NYC Open Data. Unfortunately, I ended up losing the entire east side of Manhattan, with little time to trouble shoot. Luckily, I found geojson.io, a site that lets you create your own geojson files by drawing points and polygons in browser. I found myself back in business, albeit with a more simplified neighborhood breakdown. 

After undersampling on my training data to handle the class imbalance, I ended up with the following features in an xgboost model; start station id, start station neighborhood, end station id, end station neighborhood, hour of the day, and trip duration. Using the **F1 metric**, or the harmonic mean of the precision and recall, I got an initial score of .76.  

My first models indicated that trip duration was the most important feature, and since I now had the distance between stations, I could calculate an approximate "travel speed" for each trip. It turns out that customers and subscribers tend to travel a similar distance, but subscribers take much shorter trips at faster rates.  Interestingly enough my F1 score jumped to .83 after including these features - particularly the speed.

![duration](/images/citibike/feature-importance-gain.png)

The extreme difference in average trip duration and average speed between the two classes tells us that a LOT of subscribers are using Citibike for purposeful movement, as we might expect. However, I was surprised to see that customers took much longer trips than subscribers - I was expecting more subscribers to be like myself, a commuter, and have a bit of a longer ride. Instead, my findings tell me that subscribers are either Manhattanites who don't travel far, or people who commute _into_ Manhattan by some other mode of transport. The fact that Pershing Square at Grand Central is the most popular station amongst subscribers backs up this idea.

| ![duration](/images/citibike/distance.png) | ![duration](/images/citibike/duration.png) | ![duration](/images/citibike/speed.png) |

(Note that duration is in seconds, and distance is the euclidean distance between two points.)

### How Can Citibike Appeal More to Tourists?

The data tells us that "customers," or tourists, are sticking to a few obvious areas. In order to increase sales to this group, Citibke should expand the stations near quintessential landmarks, or add more stations. I would like to look into the status of these customer-centric station over time and see if they are ever completely full - perhaps they would be good candidates for the "bike valets" that the company employs to manage bike overflow at popular stations during popular times. My other piece of advice, partially self-motivated but also data-driven if you look closely at the map, would be to increase stations along scenic areas such as western facing shoreline. There are very few stations along the Hudson River Greenway downtown - which is the perfect place to catch a sunset ride.







