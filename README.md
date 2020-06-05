# Covid19India Dashboard Analysis

**GEOG 458: Final Essay**

**GitHub repository:** https://github.com/covid19india

## Introduction

This dashboard is a crowdsource prohect that aims to track the spread of nCOVID-19 in India and the extent of the outbreak in the country. The home page primarily shows the spread of cases in India as a choropleth with a table that lists the number of cases by state and district.

I chose to analyze this dashboard for a couple of reasons. First, it is completely volunteer driven and open source, which means that this is a community response to the disease and shows how dashboards and mapping can help the community directly. Secondly, I really like how well-maintained and active it is. Given, the disease is new and widely spreading. A dev community response like this ensures that the ensures that the webapp evolves as the need evolves. Although, the project started as a dashboard only to track the disease, it now also lists places where people can buy essential commodities.

The dashboard is created and maintained by "a group of dedicated volunteers". The primary function of the webapp is in **Home** tab, it is to show the spread of the cases and also to keep a track of the spread of the disease by mapping the spread between patients in the **Demographics** tab. Apart from that there is a **Essentials** tab that shows users where they can find essentials based on their current location.

The target audience for the app is the general population in India and policymakers if they choose to use the website to get their statistics. The app works well for the target audience as it has everything related to the disease and the resources they might need.

## System Architecture

The architecture for this website is very interesting. Although, there is a whole different repository for the backend API, I will talk briefly about that and limit our conversation to the client side and the services to reduce complication.

### Data

The data for the website is gathered by volunteers. Volunteers collect data from predefined, trusted sources released by different states about the patients and new cases everyday. Volunteers then update the internal Google Sheet which acts as the primary database for the for all unidentifiable patient records.

### Server

The Client (dashboard) pulls data from an [API](https://github.com/covid19india/api) made by the same organization. This API provides several endpoints (or functions) for the client to get the data from. Different parts of the webpage use different endpoints.

The server picks up its data from the Google Sheet updated by the volunteers using GitHub Actions periodically. The period of time is the trigger event for the action which is roughly once every 10 mins as seen from the [GitHub Actions](https://github.com/covid19india/api/actions). The action is defined in a [YAML file](https://github.com/covid19india/api/blob/master/.github/workflows/javascript.yml). It specifies that the action runs on an Ubuntu virtual environment. The action uses secret tokens to interact with the Google Sheets API and runs a [bash script](https://github.com/covid19india/api/blob/master/main.sh) which in turn runs all the data fetching functions defined inside the src folder as JS files. These files have functions to convert the CSV data to JSON, organize it statewise and add geocodes for states + districts into the JSON. The bash script runs a final sanity check that updates the database on GitHub if there are a predefined number of updates.

After the sanity check, the data is saved in the gh-pages branch of the repository where the server fetches data. The clients can use this data as defined by the API for their applications. As mentioned in the [JSON table](https://github.com/covid19india/api/tree/master#json) on the README page of the server repo, there are various endpoints provided by the API. These endpoints give data at various lebels about patients and cases. They also have data about different zoning levels for the districts and about essentials.

### Services

On the backend the app majorly uses GitHub Actions and Google Sheets API. Apart from that there is Immer and MomentJS libraries used for the code to make immutable states and to use date and times properly. The webapp also uses a lot of services on the frontend. The frontend client (dashboard) uses the backend API as a service. It also uses Google Fonts for different kinds of text on the client. There is also usage of Google Tag Manager to collect analytics about the usage of the website. For mapping, the website uses state, district and country boundary GeoJSONs derived from OpenStreetMap and D3 library to show those regions as a choropleth.

### Client

The client side is made using the React framework which allows for reusable components and easier data flow throughout the app. The client side uses D3 library to visualize the map and for other charting elements as well. The client features multiple tabs for different resources, alongwith a homepage that displays the statistics of the disease using spread trends with uniform and logarithmic graphs. alongwith a map of India that displays the choropleth. This map can also be switched to show hotspots in the country using a bubble map. Clicking on a state on the map also shows the various districts inside the state and their statistics. This is also bound with the table of statistics on the side as the state's cell opens up. The frontend uses a lot of different libraries. It uses Leaflet for distance mapping for resources. It also uses Spring for state change animations and hover effects on the map.

## Code Inspection

- talk about network flow (kinds of resources, time to live, first paint)
- other libraries used
- major visual components
- functions

## Data Sources

## BaseMap

## Web Map Element

## Strengths and Weaknesses

## Interesting Observations

While researching this project and looking at various code sources, I came across a few interesting observations.

- In the repository for the frontend code I found that GitHub uses Leaflet tile panes to show a basemap when a GeoJSON fule is uploaded. For example, this district boundaries for the state of [Maharashtra](https://github.com/covid19india/covid19india-react/blob/master/public/maps/maharashtra.json) are shown in blue while an OpenStreetMap basemap is used and it is shown on a MapBox container.

## Reflection

roughly cover:

- languages
- locale translations
- patients confidential but map relations
- roadmap

Links:
https://www.covid19india.org/
https://github.com/covid19india
https://www.covid19india.org/about
https://www.mohfw.gov.in/
