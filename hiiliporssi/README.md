### Hiilipörssi data

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