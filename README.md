## Vector Data Source

*This is a work in progress towards OSM2VectorTiles v3.0*

This is the data source for the vector tile schema of OSM2VectorTiles.
It contains the *tm2source* project and the required database schema (views, functions).

The vector data sources stands as separate repository to foster collaboration with Wikipedia
and make it easier to fork the style without forking OSM2VectorTiles as well.

## Vision

The vector tile schema will contain the necessary features for creating a basemap.
We will orient ourselves on the cartography used in the [Carto Basemaps](https://carto.com/location-data-services/basemaps/).
Additional layers can be always be mixed in later on but we think OSM2VectorTiles should become less bloated in v3.0.

![Future basemaps based on OSM2VectorTiles v3.0?](./basemap_vision.png)

## Requirements

This vector tile schema depends on a database containing several different data sources
which need to be imported first. You can use your own ETL process or use the Docker containers from
OSM2VectorTiles.

Your PostGIS database needs the following data imported

- [OpenStreetMap](http://wiki.openstreetmap.org/wiki/Osm2pgsql) data based on the [ClearTables osm2pgsql style](https://github.com/ClearTables/ClearTables)
- [OpenStreetMapData](http://openstreetmapdata.com/) split and simplified water polygons
- [Natural Earth](http://www.naturalearthdata.com/)

## Schema

The vector data source is using zoom level views for each layer and contains useful functions.
The PostgreSQL code can be found in `sql`.

*TODO: Write import container*

## Develop

To work on *osm2vectortiles.tm2source* we recommend using Docker.

- Install [Docker](https://docs.docker.com/engine/installation/)
- Install [Docker Compose](https://docs.docker.com/compose/install/)

### Prepare the Database

Now start up the database container.

```bash
docker-compose up -d postgres`
```

Import water from [OpenStreetMapData](http://openstreetmapdata.com/).

```bash
docker-compose run import-water
```

Import [Natural Earth](http://www.naturalearthdata.com/) data.

```bash
docker-compose run import-natural-earth
```

### Work on Vector Tile Schema

Run the `db-schema` container each time you modify SQL code inside `./schema`.

```bash
docker-compose run db-schema
```

To work on the *data.yml* you can start up Mapbox Studio Classic in a container
and visit `localhost:3000` and open the vector source project under `projects/osm2vectortiles.tm2source`.

```bash
docker-compose up mapbox-studio
```

![Develop on OSM2VectorTiles with Mapbox Studio Classic](./mapbox_studio_classic.gif)
