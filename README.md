# [King County GIS Service’s Voter Turnout by Precinct Web Map](https://kingcounty.maps.arcgis.com/apps/MapSeries/index.html?appid=2407ec4e03bf4b8ab052edc183fe1177)

![](imgs/scrshot2.png)

The King County GIS Center releases a hybrid smart dashboard-choropleth web map that tracks voter turnout by precinct on a year-by-year basis. Currently it contains data for the years 2010 through 2019 which you can select via a navigation bar at the top of the viewer. By selecting a year, the web page overlays the data visualization for the election of that year over the base map which extends slightly past the state. The voter turnout data is visualized into polygons that follows a categorized color ramp according to the percent turnout of that precinct. The legend maintains the same color ramp ranking categories by percentage range between years to give a sense of how voter turnout changes between elections. A window to the side of the page provides the overall county turnout for that year along with a brief explanation giving context for the turnout and other links to various King County election resources and services. The user can then click on a precinct to trigger a pop-up element that provides additional, more granular data about the precinct in question such as the precinct’s legislative, congressional, and county council district number as well as the number of registered voters for that precinct compared to the number of ballots counted for the election in question.

![](imgs/scrshot3.png)

The purpose of the project and of the larger King County GIS service center in general is to provide the most recent geographic data for use by the King County regional government and for other, outside parties such as enterprises or community organizations. In the case of the voter turnout map, its main purpose is likely for administrative purposes to identify areas of voter engagement with one of the stated goals of the map being how a person can improve voter turnout for their precinct. There is the added benefit that community-level organizers can identify problem areas of low voter engagement and turnout and attempting to reach out within these communities to increase engagement year by year and for party establishments to identify additional possibly undecided voters. This mapping project was authored by King County Elections’ GIS Specialist Katrina Sroufe using data provided by the King County Elections Department and the Secretary of State from past elections’ results.

### System Architecture

The web-map application is built using ESRI’s ArcGIS platform which is hosted on an Amazon web server located in Ashburn, Virginia.


![A screenshot identifying the location of the Amazon server hosting ArcGIS web services with the command prompt ping showing the linked ip address and url](imgs/2021-03-08(2).png)
###### A screenshot identifying the location of the Amazon server hosting ArcGIS web services with the command prompt ping showing the linked ip address and url

This web server reaches out to the geospatial server then the various file and database servers to retrieve the content that is sent to the client’s browser to load and display. The web map application page hosts the main index content along with some starter CSS style sheets and JavaScript files. Elements such as the base map and King County election specific pictures and content are hosted on other domains and servers such as the county’s own servers in the Seattle area.

![A screenshot showing the file structure of the King County Voter Turnout web map application’s associated web page](imgs/scrshot4.png)
###### A screenshot showing the file structure of the King County Voter Turnout web map application’s associated web page

The primary map element is defined as an empty division within the web page’s index and then injected with content using JavaScript scripting that is first referenced within the starter files but is hosted on a separate file server which contains the ArcGIS API. This primary JavaScript contains listener methods that pay attention to the clients’ actions on the web page such that when a change is made it can request and load the necessary content such as additional base map tiles for a different zoom level or the voter turnout data for the selected election year.

### UI Design and Representation

The base map used by King County’s Voter Turnout map is actually developed especially for use by the King County GIS Center and derivative services as evidenced by the file structure of the web page. It is saved as a map tile layer of pictures that are layered below the thematic layers (to be discussed later in this paper). In addition to the standard practice of having several tile-sets visible at different zoom levels, the base map tile layer seems to be composed of three different map styles that vary between where the viewport is geographically. There is one map style that appears to be confined to the King County area.

![A screen shot of the various map styles used in the King County generic base map](imgs/scrshot5.png)
###### A screen shot of the various map styles used in the King County generic base map

This map style seems to contain fairly detailed information regarding locations, roads, and granular city level data when zoomed in. This is where the thematic layer of the choropleth map is overlaid onto, so the purpose of the increased geographic detail is for us to identify the geographic barriers that demarcate and delineate the extent of the individual precincts.

![The limits of voting precinct SEA 37-1930 in the Beacon Hill neighborhood and the streets that define its shape](imgs/scrshot6.png)
###### The limits of voting precinct SEA 37-1930 in the Beacon Hill neighborhood and the streets that define its shape

Another map style is dedicated to the rest of Washington state and its counties. This style is devoid of almost all geographic information except major bodies of water, freeways, and county lines. While these details are unnecessary to deciphering the map’s meaning the inclusion of roads likely helps the client situate King County alongside its neighbors and also defining the limits of Washington state.

![A screenshot displaying the neighboring Snohomish county which is styled with much less geographic detail than the focus of the map, King County](imgs/scrshot7.png)
###### A screenshot displaying the neighboring Snohomish county which is styled with much less geographic detail than the focus of the map, King County

The final map style compressed into the base map is completely devoid of geographic information, names, features, or otherwise. It covers the territories immediately outside Washington state such as Canada, Idaho, and Oregon before the limits cut the base map off, limiting the map’s extent to Washington state and parts of its neighboring territories. This makes it clear that those territories are not the focus of the map and attempts to lead the user back to King County with its increasing level of detail.

![A screenshot depicting the territory outside of Washington state. It is devoid of all geographic features except for state lines](imgs/scrshot8.png)
###### The territory outside of Washington state. It is devoid of all geographic features except for state lines

The thematic layer is a mildly transparent choropleth map depicting the rate of voter turnout with a color ramp with value classes: 0-30%, 31-40%, 41-50%, 51-60%, 61-70%, and 71-100%. The turnout rate is calculated by dividing the number of ballots counted by the number of registered voters inside the precinct. While the choropleth style is very helpful at understanding voter turnout at a broad level (see below):

![Two voter turnout maps: one of 2016 and the other of 2017. From a top level it is clear that voter turnout is reduced in odd years while presidential election years maximize voter turnout](imgs/scrshot9.png)
![](imgs/scrshot10.png)
###### Two voter turnout maps: one of 2016 and the other of 2017. From a top level it is clear that voter turnout is reduced in odd years while presidential election years maximize voter turnout

They are not helpful at making decisions about where best to divert resources to increase voter engagement. What the choropleth map obscures is the actual number of registered voters per precinct going so far as to misrepresenting the turnout of some districts. Furthermore, the use of choropleth representation overrepresents large areas with small populations over small, high density areas as seen in the figures below.

![Two screenshots of turnout in the 2016 election: the first of the larger Snohomish Pass voting precinct containing only 222 registered voters and the other of SEA 43-1362 in Wallingford with 842 registered; nearly four times as many registered voters!](imgs/scrshot11.png)
![](imgs/scrshot12.png)
###### Two screenshots of turnout in the 2016 election: the first of the larger Snohomish Pass voting precinct containing only 222 registered voters and the other of SEA 43-1362 in Wallingford with 842 registered; nearly four times as many registered voters!

These are also not static populations nor does the number of registered voters capture the entire body of people who are eligible to vote. For example, the number of registered voters from one precinct in the city of Tukwila grew to over twice as much in the 2010-2019 period as evidenced in the pop-up element when a precinct is clicked on. While this growth in registered voters could be attributed to a growing population it is likely equally attributable to the mobilization of a population that had already existed within the area via outreach and community organization.

The sidebar element of the web map application expresses the overall voter turnout in the state as a percentage which varies year by year – implicit within that inclusion is the desire to maximize not only during presidential election years but in the interim with advisory elections and local measures. The sidebar then links to various resources to help individuals and communities improve voter turnout with an added picture or video meant to help inspire users to engage. While this is a valid and legitimate call to action that can help improve voter turnout, it places undue onus on the voting populace to turnout instead of focusing on structural practices that can improve voter turnout that remain in the responsibility of our civic institutions to implement and suggest.

On the responsive design front, the application is more than capable of loading on a variety of smartphones and tablet devices. At the same time, it is not perfect, much of the text is truncated by the small size of phone screens making certain titles and election years less-than-ideally legible. One particular problem that occurs on larger phone screens such as the iPhone X or the Pixel 2 XL happens when those phones are turned to landscape view where the sidebar (which is not present on smaller screens) cramps the available map real estate alongside the legend which reduces legibility of the application.

![Screenshots of the application as it shows up on different devices. (a) shows that the application shows up in near identical form on tablets. (b) shows the application on a smartphone, there the sidebar and legend is excluded while the navigation bar is replaced with a carousel that allows the user to cycle through election years. (c) shows the application as it appears in landscape view on larger mobile phones, the map body is cramped by the presence of both the side bar and the legend.](imgs/scrshot13.png)
![](imgs/scrshot14.png)
![](imgs/scrshot15.png)
###### The application as it shows up on different devices. (a) shows that the application shows up in near identical form on tablets. (b) shows the application on a smartphone, there the sidebar and legend is excluded while the navigation bar is replaced with a carousel that allows the user to cycle through election years. (c) shows the application as it appears in landscape view on larger mobile phones, the map body is cramped by the presence of both the side bar and the legend.
