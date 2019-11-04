# Improve Travel Planning to Sporting Events for Coaches, Fans, and Families

# 1. Introduction   
## 1.1 Background   

Youth, high-school, and amateur sporting events take place in many locations.    Friends and family members of athletes will often travel to towns and cities they are not familiar with to watch and support the athletes. For example, a typical high-school basketball team will play roughly half its games “on the road” - away from the “home” location, gymnasium, field, etc.  These trips are rarely longer than a single day, and in many cases last only a few hours.  Everyone is busy - parents,  kids, siblings, friends, athletes, and the coaches - and with such short trip durations, extensive planning is frequently not worth the effort.   


## 1.2 Problem

Coaches and athletes would like friends and families to have enjoyable experiences when they travel to support athletes. The coach is frequently the center point of all the communication to athlete and athlete’s families, and as a result may get frequent questions similar to  “Where’s a good place to eat after the game ?”. The problem is that coaches don’t have a simple way to provide a “local guide” for each location on the schedule, and usually don’t have the staff or time available to do this manually.   

The core questions from a coach's perspective:  
(1) Which locations on the schedule have the most options and least options for friends and families ?  Specifically for food, coffee, snacks, etc - services that would be useful for friends and families either before or after a sporting event.

(2) Can each location be labeled in a way to indicate if additional planning is prudent or pragmatic?  These labels would convey total options available at given location to help friends and families in their travel planning.  If there are many options, little additional planning needs to be done; If there are few options, significant additional planning might be needed.  

(3) Provide a selection of a few the most popular or highly recommended venues in each location.  

Answering these questions enables a coach to provide information to assist athletes and their friends and families in travel planning for upcoming sporting events.


## 1.3 Audience : Coaches, Fans, Families  

Organizations like prep schools, will draw families and students from a wide geographic area. Amateur endurance athletes competing in sports like marathons and triathlons will often use online coaching.  In both cases,  “local knowledge” about the “away” locations may be unavailable to the coaches, athletes and their families; leaving recommendations for simple things like "where to eat" hard to find. 

“We’ll figure it out when we get there”, is a perfectly fine strategy for locations with many food and shopping options, but for locations with very few options, this can result in an unpleasant trip. Informing friends and families which sporting event locations need additional planning will help reduce the stress associated with travel. 

# 2. Data  

## 2.1 Requirements

The following data is required:

- **Location Information** of each sporting event.
    - Data type is a text, in the form of City & State; e.g. Lawrence Academy, Groton, MA,  or  Skeetwater Triathlon, Lithia Springs, GA, or Boston Marathon, Boston, MA

- **Latitude & Longitude** for each sporting event location. 
  - Data type is a pair of decimal numbers, as in (42.604433, -71.5628436838169)  

- **Venue Information** for restaurants, shops, services surrounding each sporting event location.  
  -  Venue Name, Venue Location (Latitude & Longitude), Venue Category, Venue Parent Category.  (See Sources for information)  

- **Category Tree**  to map each individual category to a “roll-up” parent category type.  For example “Pizza Place” will roll-up to “Food”.  
  - Tree structure of all categories describing how they relate to each other.

- **Estimated Driving Distance and Duration** from “Home Location” to each event 
  - Estimated Driving Distance in meters  (will convert to other units for reporting)
  - Estimated Driving Duration in seconds (will convert to other unites for reporting)


## 2.2 Sources 

High level constraints on collecting data:  
- It must be no-cost.  There is no budget for this project, so data available from paid sources was not considered.  
- It is only North American data.  There is nothing explicitly exclusionary about the data collection for this project. However, nothing outside the Northeastern United States was included in the analysis; therefore, this analysis may not be transferrable to other geographic locations.  


- **Sporting Event Locations**  
  The data was collected from  the 2018-2019 Varsity Girls Basketball Schedule of Cushing Academy, a nearby College Prepatory School.   A custom web-scraper was built to collect the information from [the team page](https://www.cushing.org/team-detail?fromId=204845&Team=87969&SeasonLabel=2018+-+2019) 


- **Sporting Event Location Geocoding**  
  The latitude and longitude of each school location was collected from [Open Street Maps Nominatim](https://nominatim.openstreetmap.org/) geocoding service.  For Reference: [The API information](https://nominatim.org/release-docs/develop/).  
 

-  **Venue Information & Category Tree**   
    Venue information is collected from the [Foursquare API](https://developer.foursquare.com/docs)  

    The [category tree](https://developer.foursquare.com/docs/resources/categories) is also accessible from Foursquare.

    Specific data types and attributes for venues or categories can be explored in the [Foursquare Documentation](https://developer.foursquare.com/docs/api/venues/details)  


- **Routing information**  
  Routing information (Estimated Driving Distance and Estimated Driving Duration) is provided by [Open Source Routing Machine Project.](http://project-osrm.org/)


## 2.3  Data Preparation

## 2.3.1 Collection & Cleaning

This section details specific issues, decisions, and assumptions that might have impacted data collection or cleansing.  

### Sporting Event Location Data   

The Sporting Event Location data had several records that required manual data correction.  The web-scraped extraction of event location data did not include City and State designation.  As a result the geocoding service return incorrect latitude and longitude.   For example, searching for “St. George’s School”,  returns a school in England.  Even revising the search term to include country code (`US` in this example), did not ameliorate the issue.  The modified search with country code returns a school in California.

Using domain knowledge that all games for Cushing Academy are regional, the cleansing strategy was to group all sporting event locations by city and state.  Unintended results provided by the geocoding service were easily identified;  for example, results like England, North Carolina, Pennsylvania are not regional to Cushing Academy and are clearly in error.  Using Google I looked up the correct city and state for schools and modified the search term and updated the dataset with the correct latitude and longitude. 


### Venue Information 

**Limiting the search area around a location.**   
The search radius around each location is 2.5 miles.  

This distance was chosen as how far is it likely someone can travel from location in 10 minutes.  

10 minutes was chosen based on the assumption most travelers would like to get back on the road as quickly as possible,  while also driving as long of a segment as possible on the trip home.  “Eat quick and hit the road”, if you will.  Therefore restaurants and shops should be very close to the event location.

School zones speed limits are typically in the range of 15mph to 25mph.  Since our event location list is from a High School sports schedule, most locations will also be schools. Also including that these are events - it is possible there will be hundreds to thousands of other people also attending these sporting events. Minor Congestion around the event location (parking lots, etc) might also impact the driving speeds.   Allocating 2 minutes for walking to car, getting out of the parking lot, etc, leaves 8 minutes.  At 15 mph, you can travel 2 miles in 8 minutes; at 25 mph, you can travel 3.3 miles.  While not the midpoint, 2.5 miles was chosen as a good radius to comply with “Eat quick and hit the road” desires.


**Limiting the number of venues**    
The analysis is limited to 100 venues per location.   

The judgement is that the additional information beyond 100 venues will not impact the primary purpose of the analysis, which is to indicate locations that require extra planning.  Consider the thought experiment, location A has 100 venues, location B has 3 venues.   If a game next month is at location A,  it is low-risk to “wing-it, and pick something when we get there”.  Is the relative change in risk for a location with venue counts of 100 vs 200 venues impactful ?  So long as locations such as location B are in the dataset , the difference within the same order of magnitude (100 vs 200), are insignificant compared to the changes in risk when orders of magnitude change (1 vs 10 vs 100 vs 1000). That is, the difference between 3 venue options and 100 venue options is significantly (and practically) more impactful to the families traveling than the difference between 100 and 200 venues. The purpose of the model is to indicate that locations, such as location B, will require significantly more research than location A to find a “good place to eat.”  Thus, the decision is that extra information beyond 100 venues will not impact the quality of the recommendations. 


**Category Mapping to a Parent Category**   
Foursquare provides the complete set of category information in a tree structure.    For example, if a venue is categorized as a “Multiplex”, that category is a child of the “Movie Theater” category, which is a child of “Arts & Entertainment”, which is a top-level category in Foursquare. As part of the analysis, all children categories are mapped to the top-level parent.  Continuing the example, “Multiplex” will be mapped to “Arts & Entertainment”, in effect flattening the tree, and losing the “middle child” of “Movie Theater”.  For completeness, that middle child, “Movie Theater” is also mapped to the top-level parent category.   All categories have been mapped to one of the 10 top-level categories.  


## 2.4   Representative Model / Data  

The analysis is based on a convenience sampling of one nearby Prep School.  It is possible that any conclusions learned on this data set won’t be transferrable to other locations.

In the context of a “League” or “Conference”, where multiple similar institutions compete agains each other, form youth soccer to high-school volleyball,  There are two clear situations where this convenience sampling will represent  other locations.  When those locations are:   
  1.   High Population density   
  1.  Very low population density  

For high-population density areas, there will be many opponents within a confined geographic area, so the lack of “local knowledge”  may not be an issue, as travel distances may not even take you out of the neighborhood.  In some cities, an away game, might only 20 be blocks away.  There is very little planning required when traveling 20 blocks.

For low-population density areas, it is unlikely to be surprised as everyone already knows there are few options. The expectation of having 20 or 30 dining options to choose from is not present.  

In a nutshell for “leagues” within homogenous geographic areas, either high or low population densities, the usefulness of a planning tool is greatly reduced, as it is unlikely for a location to be outside expectations. 

It is clear the approach in this analysis is not universally applicable. However, there are many situations where people choose to travel to a diverse set of locations, or the location in question is sufficiently far away that a simple tool to judge the amount of planning required before travel is useful. In my opinion, the convenience sample in this analysis does in fact represent a large population.  
  

# 3. Methodology

## 3.1 Analytic Approach  

The heart of the primary research questions might be restated as, how to describe a location that informs the planning requirements before travel. This perspective drives the analysis to be focused on descriptive models and patterns. The outcome therefore, is a way to describe multiple locations in a way that is consistent across diverse population context to enable quick decisions supporting travel planning.


## 3.2 Exploratory Analysis 

### 3.2.1  Away Location Visualization / Mapping

A basic map of the region was used to get a visual feel for the locations in the dataset are located. The high school chosen as a data source travels as far south as Rhode Island, as far West as Massachusetts border, nearing New York, and fairly far north into New Hampshire.  On the positive side there is a great diversity in this location set, very rural settings to very urban settings.

![Map of Away Games](https://raw.githubusercontent.com/mcmasty/Coursera_Capstone/master/images/Away_games_map.png "Map of Away Games")**Map of Away Games**  


### 3.2.2  Venue Counts for each Location   

The venue date set contains a 1,037 venues, for 23 locations.  The data has a right-skew.  Many locations have few venue options, while a few locations have many venue options. This right-skew might impact the clustering. 

![Venue Histogram](https://raw.githubusercontent.com/mcmasty/Coursera_Capstone/master/images/all_event_location_venues_hist_1.png  “Venues Histogram”)**Venue Histogram**  



### 3.2.3  Travel from Home Location to Away Location

There are two dimensions when analyzing travel, distance and duration.

![Travel Histogram](https://raw.githubusercontent.com/mcmasty/Coursera_Capstone/master/images/away_location_driving_hist_1.png   “Travel Histogram”)**Travel Histogram*   

The distance and duration plots seem like they will cluster relatively smoothly.  The data does not have a dominate skew right or left. No bin in the histogram has more than 5 locations. This data should not be too challenging to work with.


### 3.2.4  Clustering  

One of the goals of this analysis is to assign labels to groups of locations which indicate how many venues will be useful to travelers.  This naturally leads for a heavy bias towards “Food” venues.  

Foursquare has 10 Top-Level categories and the data has been rolled to the highest level. It is possible to then cluster on 10 features, one for each top-level category.  However, the dataset only has data for 8 top-level categories.

Knowing that “Food” will hold the most sway begs the question, will using all 8 features result in over-fitting the model ?  Or even more pragmatically, is there benefit to the added sophistication ?

The question if clustering on single venue, Top-Level Category 

Based on binning
Based on Means
Model Selection


### 3.2.5  Geodesic Distance from Venue to Event Location 


### 3.2.6  Venue Ratings



# 4. Results 

Was able to produce useful labels


# 5 Discussion  

Good start but 
- Clean-up some visual displays 


- The routing information led me to consider it might be more useful if personalize by family locations … so that each family gets a report of ‘games closest to me’

- More sophisticated filtering , Vegan, Gluten-Free (assuming Foursquare has available data sets)  (venue filtering)

- Different travel optimizations , such as ‘break the trip in half’ … or ‘longest possible segment’ … or want to see a friend on the way …  (routing option)