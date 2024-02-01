# competitive-analysis-open-data
Identify business opportunities in Vancouver through competitive analysis of open API data (Google Places, Yelp, CityBikes).

# Process

- decide on city
    - Chose vancouver due to personal familiarity
    - This analysis will primarily consist of agreegating and interpolating millions of coordinates that make up the points of interest and the contextual geometry at different scales of analysis. There are many opportunities for things to become misaligned. The capacity for error catching is significantly increased through local awareness.
- Defining Goal of Analysis: Recommending a location for a new business entrant by comparing the competitive landscape.
- decide on type of business
    - limited scope to cafe's due to several practical realities
        - cafes are ubiquitous, this ensures adequate sample size and gives flexibility to scale of analysis as I refine the project goals.
        - Cafes have less variance in customer taste preference relative to restuarants. In a pinch, "good enough" will do.
        - Google Places API has liberally applied place-category tags. The response data requires requires manual investigation to filter out inappropriately categorized places/venues. The cafe category simplifies this elimination process.
            - For the purpose of this analysis "cafe" is defined as a storefront that:
                1. serves coffee or tea
                2. Has seating
- Defining perimeter "location" of analysis
    - rapid transit stations
        - 
        - (catchment area within 500m radius or ~5 min walk from station)

- decide on method for selecting winner:
    - collect metrics on supply (cafe data)
    - collect metrics on demand (develop proxy for estimating local foot traffic)
    - find the locations with the largest supply gap (where demand is high and supply is low)
- acquiring data
    - map feature data
        - city of vancouver open data portal
            - storefront inventory
            - bus stops
            - parking meters
            - bike routes
            - neighborhood boundaries
            - neighborhood demographics (2016 census)
            - parks
            - zoning districts
        - augmented with misc data
            - translink bus stops (Abacus Open Data, UBC Library)
            - bikeshare stations (Citybikes api)
    - cafe data
        - google places api, used transit station coordinates to populate each catchment area
        - rating, price level, operating hours
- processing data
    - used python and geopandas to spatial join all geojson data
        - generated 500m buffer zone around each rapid transit station
        - assigned station_id to all intersecting map features
    - processed cafe data to enable comparison
        - parse and categorize operating hours to distinguish standard vs extended hours
- analyzing data
    - for each rapid transit station catchment area:
        - used spacial intersections to create count of supply/demand indicators
- ranking options and giving recommendation
    - used min-max normalization on all indicator data to reduce influence of outliers on conslusion
    - developed formula for scoring supply and demand
        - supply score:
            - total count of cafes
            - count of cafes with extended hours (early opens, late closes, 24hr places)
            - count of cafes at different price levels
                - more weight given to lower price levels with broader market appeal
        - demand score:
            - applying weights based on the indicators positive impact on foot traffic
                - high impact: count of storefronts, neighborhood population density
                - medium impact: bus stops, bikeshare stations, commercial and mixed-use zoning
                - low impact: bike routes, parks, residential and industrial zoning
        - supply-gap score:
            - supply - demand
            - rank catchments by this score to determine which areas have the most unmet demand, and therefore the lowest amount of competition for a potential new entrant



## Future Goals
Things I would do if I had more time:
- process and normalize venue categories to improve analysis
- 