# Neighborhoods in Toronto

The Premise: Your family wants to move from where you currently live, but they don't want to move to a neighborhood that is very different
because it's hard to get used to.

But, you can't exactly just Google "Neighborhoods like mine". And there isn't a fixed way to classify neighborhoods.

So what can we do? We can use clustering to find similar neighborhoods without actually classifying each neighborhood.

Without further ado, let's begin

#  Step 1: Getting The Neighborhoods

Luckily, Wikipedia has a list of Postal Codes with attached Neighborhoods. Why Postal Code? This way we can actually know where each neighborhood is located with future processing. The site: https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M

In this process, I used IBM's Watson platform to scrape the tabular data off of Wikipedia and create a CSV
Also in this process, is cleaning the data so it's readable and removing any useless data, and there a quite a bit.

Postal Code was used to separate the neighborhoods because if their postal code is the same, the neighboorhoods are close to one another, which means they are similar.

To see this process:
https://dataplatform.cloud.ibm.com/analytics/notebooks/v2/14190bd8-db71-4a43-8d52-0b7eab44e9b2/view?access_token=278bfab56be1ae0059337cb3f53bad60e75a8fbbff9a346d031aee76afbd6ca2

Now we have the Neighborhoods data and it looks like this
https://github.com/Fjallen/NeighborhoodsToronto/blob/master/TorontoNeighborhoods.csv

# Step 2: Getting Latitudes And Longitudes

It isn't enough to just know the postal codes, we also need to have latitudes and longitudes to call most, if not all, APIs

In this process, I worked on my own computer since the Watson platform has restrictions on installing Packages, which might be needed
In this case, it was a Google Geograhy Package, which might be needed for geography purposes.
By working off my own computer, I have the freedom of just using "pip install" and I recommend the same when using smaller datasets.


In this case, I didn't use the aforementioned package since I googled and turned out there was an existing csv that I can just use.
I proceeded to simply merge the two tables based on Postal Code.

To see this process:
https://github.com/Fjallen/NeighborhoodsToronto/blob/master/Adding_Coordinates_To_Toronto_Neighborhoosd.ipynb

### Note that if you follow the steps on the notebook on your own computer, there should be Maps. Github just doesn't render it.

# Step 3: Getting Features and Clustering 

Now that we have the Longitudes and Latitudes, we can work on getting features for each neighborhood.

We can do this by finding the various things(venue) to do near the Latitude and Longitude coordinates. And to do this, we will be using the Foursquare API, which does just that. The free account gives more than enough API calls to complete this. You will need your own account if you want to use it.

The foursquare API already tells you the type of establishment the venue is, ex. Asian restaurant, and we will be using that.

Of course, we have to parse the response back into the data after we receive it.
We will be using one-hot encoding which you can visualize here: https://www.google.com/search?q=one+hot+encoding&rlz=1C1CHBF_enCA752CA752&source=lnms&tbm=isch&sa=X&ved=0ahUKEwjng4betpHiAhVH4qwKHV1sCjsQ_AUIDigB&cshid=1557507318600509&biw=1920&bih=969#imgrc=lyAsLJtlYs1DpM:

During this process, we find a few neighborhoods without any venues, and it turned out that there were some very boring neighborhoods, and one neighborhood was a ZOO! We can just drop these neighborhoods since we won't be moving to them.

With some further cleaning, and getting the most popular venues, we finally got the features we can use for the clustering.

We will be using K-Means Clustering to form the different clusters of similar neighborhoods.

And voila! We have groups of similar neighborhoods to consider moving to.

### Note that the K (number of clusters) in the current notebook is 10, which is arbitrary and can be lowered, I simply moved from 3 to 10, by 1s to see how things changed.

To see this process:
https://github.com/Fjallen/NeighborhoodsToronto/blob/master/Toronto_Neighborhood_Clustering.ipynb

The finished CSV: 
https://github.com/Fjallen/NeighborhoodsToronto/blob/master/Clustered_Neighborhoods.csv

# Findings

It is quite difficult to actually save data as csv in Watson Studios.
There was a large chunk of similar neighborhoods which I thought would disperse if I increased K, however, this was not the case
And it turned out:
There are a LOT of East-Asian Neighborhoods, Seconded By Italian-esque Neighborhoods, The rest was more random.
