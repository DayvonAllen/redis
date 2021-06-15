## Geospatial
- Used for storing data related to location like latitude and longitude.
- In Redis, for each lat and long pair, a GeoHash is computed.
- GeoHash - a 52-but integer value in Redis. In the real world a GeoHash encodes positions in a short string of letters and digits.
- Each GeoHash is stored in the named key. The Data type of that key is a sorted set.
- The GeoHash is stored as the score, and the name of the point is used as the value of the member.
---

## GEOADD
- `GEOADD` - Adds the specified geospatial items (latitude, longitude, name) to the specified key.
  - Data is stored into the key as a sorted set.
  - There are limits to the coordinates that can be indexed: areas near the poles(north and south) are not indexable.
  - Valid longitudes are from -180 to 180 degrees.
  - Valid latitudes are from -85.05112878 to 855.05112878.
  - In the worst case scenario the error may be up to 0.5%.
---

## Commands
- `geoadd <key> <long> <lat> <member name(usually just the place of the coords)>` - you can add multiple long and lat pairs at once.
  - You have to use the sorted sets commands to work with geospatial data.
- `geohash <key> [member...]` - returns the geohash representation of the long and lat pair in redis.
- `geopos <key> [member...]` - returns the lat and long of the member
- `geodist <key> <member1> <member2> <unit>` - returns the distance between the two members in the specified unit
  - `unit` = `mi` for miles, `km` for kilometers, `m` for meters , `ft` for feet.
- `georadius`
- `georadiusbymember <key> <member> <radius(m|ft|km|mi)> <withcoords(optional, returns coors)> <withdist(optional, returns distance)> <withhash(optional, returns hash)> <count(optional, returns count)> <ASC|DESC(optional)> <STORE key(optional)> <STOREDIST key(optional)>` - you can fetch members based on their radius
  - `georadiusbymember mypoints "My Park" 10 mi withdist` - Gets every from my geospatial points where their lat and long show that they are 10 miles away from the lat and long of "My Park"(Which is the central point) and prints the distance between them.