# Coursera Capstone, IBM Data Specialization
## Course 9, Week 4
### "Battle of the Neighborhoods"

***  

#### Part 1.	A description of the problem and a discussion of the background.


Youth, high-school, and amateur sporting events take place in many locations.    Friends and family members of athletes will often travel to towns and cities they are not familiar with to watch and support the athletes. For example, a typical high-school basketball team will play roughly half its games “on the road” - away from the “home” location, gymnasium, field, etc.  These trips are rarely longer than a single day, and in many cases last only a few hours.  Everyone is busy - parents,  kids, siblings, friends, athletes, and the coaches - and with such short trip durations, extensive planning is frequently not worth the effort.   

Organizations like prep schools, will draw families and students from a wide geographic area. Amateur endurance athletes competing in sports like marathons and triathlons will often use online coaching.  In both cases, the crux is that “local knowledge” about these “away” locations may be unavailable to the coaches, athletes and their families; leaving recommendations for simple things like "where to eat" hard to find.  

Coaches would like friends and families to have enjoyable experiences when they travel to support athletes. The coach is frequently the center point of all the communication to athlete and athlete’s families, and as a result may get frequent questions similar to  “Where’s a good place to eat after the game ?”.
The problem is that coaches don’t have a simple way to provide a “local guide” for each location on the schedule, and usually don’t have the staff or time available to do this manually.   

The solution is for a coach to be able to provide a schedule of events, and get a “local guide book” for each location on the schedule; listing popular and trending venues in the area.  With a categorization  for each location to indicate if the friends and families need to do additional planning, because the location does not have many options.  The coach can provide all this information to the athletes and their friends and families to help them plan out the travel related to the upcoming events.

***  

#### Part 2: A description of the data and how it will be used to solve the problem.

This focus will be on events held in the USA.

The core data is as follows:

  - Location Information of each event as City & State; e.g. Lawrence Academy, Groton, MA,  or  Skeetwater Triathlon, Lithia Springs, GA, or Boston Marathon, Boston, MA
  - Latitude & Longitude for each location
  - Explore all Foursquare Venue information in proximity for each Latitude & Longitude  
     - Additional information for Trending venues
     - Calculate the average rating for restaurants to feed into location category
  - Location category based on the number and rating of venues inside the search radius
