## Hiilipörssi data

The data is in two separate parts:
* Polygon shapefiles in their separate folders that contain coordinate outlines
* Excel sheet that contains rest of the information on the objects


#### Polygon shapefiles to map tiles

Run merge_data.ipynb

Create a centroid for each feature:

    ogr2ogr -sql "SELECT ST_Centroid(geometry), * FROM data" -dialect sqlite data/points.shp data/data.shp

Translate coordinates:

    ogr2ogr -f GeoJSON data/polygons.geojson data/data.shp -s_srs EPSG:3067 -t_srs EPSG:4326
    ogr2ogr -f GeoJSON data/points.geojson data/points.shp -s_srs EPSG:3067 -t_srs EPSG:4326

Create tiles:

    tippecanoe -e data/tiles --force -zg --no-tile-compression polygons.geojson points.geojson

Transfer to server:
    
    scp -r data/tiles avoin@server.avoin.org:~/data/map/hiiliporssi


#### Excel sheet to json

Run xlsx_to_json.ipynb

Transfer to server:
    
    scp -r data/hiiliporssi.json avoin@server.avoin.org:~/data/map/hiiliporssi