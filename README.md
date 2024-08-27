# escuela-del-aire
## Basic Server utils (BETA)
http://localhost:8081/qgisserver?%20%20MAP=/home/qgis/projects/world.qgs&%20%20LAYERS=countries&%20%20SERVICE=WMS&%20%20VERSION=1.3.0&%20%20REQUEST=GetMap&%20%20CRS=EPSG:4326&%20%20WIDTH=400&%20%20HEIGHT=200&%20%20BBOX=-90,-180,90,180


http://localhost:8081/qgisserver?MAP=/home/qgis/projects/world.qgs&LAYERS=countries&SERVICE=WMS&VERSION=1.3.0&REQUEST=GetMap&CRS=EPSG:4326&WIDTH=400&HEIGHT=200&BBOX=-90,-180,90,180


## Activar entorno virtual
source ~/entornos-virtuales/qgis-env/bin/activate

## directorio de plugins
escueladelaire
Environment="QGIS_PLUGINPATH=/home/escueladelaire/qgis-plugins"

## revisar logs de qgis server
sudo journalctl -u qgis-server.service -f
## revisar logs de nginx
sudo tail -f /var/log/nginx/error.log

## habilitar la variable de entorno QGIS_SERVER_LIZMAP_REVEAL_SETTINGS con el valor True 
sudo nano /etc/systemd/system/qgis-server.service
[Service]
Environment="QGIS_SERVER_LIZMAP_REVEAL_SETTINGS=True"
