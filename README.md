# Regional Park Accessibility in the San Francisco Bay Area
*An accessibility analysis of the East Bay Regional Park District*

## 1. Problem Statement
Public parks are not just a luxury, but a necessity. Like schools, clean water, and other forms of critical infrastructure, local parklands provide benefits that are essential to the health, climate, and economy of their surrounding communities. However, recent research shows that access to parks remains highly inequitable in major metropolitan areas across the United States. For example, a recent study by The Trust for Public Land found that people living in predominantly minority and low-income neighborhoods have access to over 40% less park acreage on average than residents of predominantly white and high-income neighborhoods.[^1]

The present study analyzes the accessibility of a particular park system, the East Bay Regional Park District (EBRPD). Located to the east of the San Francisco Bay in Northern California, EBRPD comprises 73 parks spanning 125,000 acres across Alameda and Contra Costa counties. Unlike the local city parks that are the focus of most park accessibility studies, the regional parks of EBRPD are not simply recreational greenspaces embedded within urban neighborhoods, but rather pockets of open wildlands that surround and separate the dense urban communities of the SF Bay Area. Thus, although one of the main goals of EBRPD is to provide access to these rich natural resources for the 2.8 million residents living within its two-county service area, the park district also has an equally important duty to protect and preserve these lands from human impact at the same time.[^2]

The distinctive nature of EBRPD’s regional park system calls for a special approach to assessing its accessibility. The usual method of counting how many people live within a 10-minute walk from their nearest local city park would not be an appropriate measure for EBRPD. The primary mode of access to the more remote regional park system is not on foot but by car. However, car ownership itself is a major source of social inequality in East Bay communities. Instead of walking or even driving, this study therefore focuses on accessibility via public transit. Transit is the longer-range mode of transportation that is most available and affordable to those East Bay residents who are also most likely to have difficulty accessing the regional parks by car. Moreover, transit is also a more sustainable mode of transportation that aligns with EBRPD’s conservationist mission to mitigate climate change and protect the Bay Area’s natural resources. The main objectives of this study are therefore to evaluate the accessibility of EBRPD’s park system via public transit and identify the neighborhoods within its service area that have the greatest need for better accessibility.

The study combines administrative data from the EBRPD itself with census data from the American Communities Survey and transportation data from local transit agencies and OpenStreetMap. These data sources are integrated and analyzed using open-source Python libraries including GeoPandas, Folium, UrbanAccess, Pandana, and OSMnx. The resulting analysis identifies over one hundred neighborhoods within the EBRPD’s two-county service area in need of better transit access to the park system.

## 2. Data & Methods
The foundational data used in this study is a series of GIS files containing the geographic areas of the parks in the EBRPD system and the precise locations of their entrances. This internal administrative data was generously provided by officials at the EBRPD and is presented in this study with their permission.[^3]

This study uses open-source Python libraries from the Urban Data Science Toolkit (UDST) developed by UrbanSim to analyze the accessibility of the parks.[^4]  First, it uses the UrbanAccess library to build a model of the regional transportation network in EBRPD’s two-county service area [^5].  It acquires transit data from the GTFS feeds of local transit agencies and pedestrian data pulled from the OpenStreetMap API, then integrates these into a single network of origins and destinations connected by edges weighted for travel time.[^6]

Next, the study uses another UDST library called Pandana to calculate the shortest travel times from each point in the network to its nearest park entrance.[^7] I chose to use travel times from 11am to 2pm on Saturdays, because this is when people are most likely to access a regional park. I also aggregated the travel times by census tract, resulting in an average travel time from each local neighborhood in the EBRD’s service area to the closest regional park via walking and public transit.

Finally, the study calculates each neighborhood’s level of need for better accessibility based on both travel time and vehicle availability. First, neighborhoods where the average travel time to the nearest park was over 30 minutes were raised one level of need for better accessibility. Second, neighborhoods where the percentage of households without a vehicle available was above the regional average were also raised one level need for better accessibility.[^8] This resulted in a metric with three need levels from 0 to 2, with 0 being the lowest level of need and 2 the highest.

## 3. Results & Analysis

With respect to travel distance, this study found that 62% (385 out of 621) of neighborhoods (i.e. census tracts) in the EBRPD’s two-county service area are over 30 minutes away on average from the nearest regional park via walking and public transit on a Saturday between 11am and 2pm. The majority of such low-transit-accessibility neighborhoods are located inland in the eastern portion of the service area where both population density and transit coverage are more sparse. However, there are also some neighborhoods with poor transit accessibility to regional parks that are located in the more urbnized and transit-rich areas along the western coastline.
<img src="/img/travel_dist.png"
alt="Travel Distance"/>

With respect to car ownership, 35% (180 out of 621) of neighborhoods in the service area have a percentage of households with no vehicle available that exceeds the regional average of 8%. These are the neighborhoods that are most likely to rely on public transit to access the regional park system. The majority of such low-car-accessibility neighborhoods are located in the densely-populated western coastal portion of the service area. However, some of these neighborhoods are also located inland as well.
<img src="/img/vehicle_availability.png"
alt="Vehicle Availability"/>

When it comes to the overall level of need for greater accessibility, the two metrics of travel distance and car availability largely cancel each other out. In other words, the neighborhoods that are located more than 30 minutes away from the nearest regional park by transit are also the neighborhoods where households are mostly likely to have access to a car. However, this study found 117 neighborhoods where both travel distance is high and vehicle availability is low. These neighborhoods (represented on the map below in red) have the greatest need for better accessibility to the regional park system via public transit. The majority of these 117 neighborhoods are located in coastal cities like Oakland and Richmond, but a few are also located in inland cities such as Antioch and Concord.
<img src="/img/need_level.png"
alt="Need Level"/>

The results described above can be explored in greater detail on this interactive map:
<br><br>
<a href="maps.park_access_map.html">
<img src="/img/interactive_map.png"
alt="Interactive Map"/>
</a>

## 4. Conclusion & Next Steps
This study identifies 117 neighborhoods in need of better transit access to the EBRPD’s regional park system. Households in these disadvantaged neighborhoods live on average more than half an hour from the nearest regional park by public transit and are also less likely to own a car than most other residents of Alameda and Contra Costa. It is the conclusion of this study that future efforts to increase the overall accessibility of the regional park system should focus on reducing the travel time for households in these 117 currently disadvantaged neighborhoods.

However, this study suffers from a number of limitations that could be improved with the following steps:
* First, the travel times were only calculated for a three-hour window on a single day of the week. It would be better to aggregate travel times throughout the week, possibly incorporating park usage data to develop weighting for peak and off-peak hours.
* Second, travel distances were only calculated to the official entrances of the parks, though it is possible to enter many of the parks at other locations, particularly if entering on foot. A more accurate picture could be developed if these unofficial entrances could be captured, such as by creating buffers around the park areas and adding access points where they intersect with the street network.[^9]  
* Third, there are missing values for vehicle availability in the ACS dataset that excludes some neighborhoods from being identified as having the highest level of need. These neighborhoods could be more fully incorporated into the study by interpolating their missing with geographically weighted spatial regression.
* Fourth, this study only measures accessibility via walking and transit. A more complete analysis could be made by adding driving, biking, and ride-sharing to the modalities considered.
* Fifth, this study is focused on identifying neighborhoods in need of better accessibility to the entire park system. However, from the perspective of the EBRPD, it may also be useful to calculate the accessibility of individual parks from residents throughout the service area. This might help to identify which particular parks are in need of accessibility improvements.


[^1]: The Trust for Public Land, *Parks and an Equitable Recovery* (San Francisco, CA: 2021).
[^2]: East Bay Regional Park District, *2013 Master Plan* (Oakland, CA: 2013). For more information on EBRPD, see https://www.ebparks.org/.
[^3]: I'm particularly grateful to Brian Holt and Kara Boettcher at EBRPD for sharing this data with me and also helping me understand the issues around accessibility to the park system.
[^4]: On the Urban Data Science Toolkit and UrbanSim, see https://urbansim.com/udst.
[^5]: Samuel D. Blanchard and Paul Waddell. 2017. "UrbanAccess: Generalized Methodology for Measuring Regional Accessibility with an Integrated Pedestrian and Transit Network." *Transportation Research Record: Journal of the Transportation Research Board*. No. 2653. pp. 35–44.
[^6]: Data was collected from the following transit agencies: Bay Area Rapid Transit (BART), AC Transit, WestCat, County Connection, and Tri Delta.
[^7]: Pandana calculates accessibility metrics using a contraction hierarchies algorithm; see https://github.com/UDST/pandana.
[^8]: The census data on household vehicle availability was drawn from the American Communities Survey (ACS) table B08201.
[^9]: Trust for Public Land, Access to Pennsylvania s Outdoor Recreation Areas, p. 5. https://www.dcnr.pa.gov/Recreation/PAOutdoorRecPlan/Documents/PA-Outdoor-Rec-Access-Report-Trust-Public-Land.pdf
