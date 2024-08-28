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
## PGTUNE
pgtune -i /etc/postgresql/16/main/postgresql.conf -o /etc/postgresql/16/main/postgresql.conf.pgtune --type Web

## despues de crear /etc/postgresql-common/pg_service.conf para el acceso a la base de datos de lizmap, crear estos permisos
sudo chown www-data:www-data /etc/postgresql-common/pg_service.conf
sudo chmod 640 /etc/postgresql-common/pg_service.conf

## iniciar qgis server
sudo systemctl start qgis-server

## Nota asegurarse que www-data tenga acceso a la ubicacion donde estan los plugins
sudo -u www-data ls /home/escueladelaire/qgis-plugins

## Ejemplo de configuracion para iniciar fcgi
``` ini
[Unit]
Description=QGIS server
After=network.target

[Service]
;; set env var as needed
Environment="LANG=es_CO.UTF-8"
Environment="QGIS_SERVER_PARALLEL_RENDERING=1"
Environment="QGIS_SERVER_MAX_THREADS=12"
Environment="QGIS_SERVER_LOG_LEVEL=0"
Environment="QGIS_SERVER_LOG_STDERR=1"
Environment="QGIS_PLUGINPATH=/home/escueladelaire/qgis-plugins"
Environment="QGIS_SERVER_LIZMAP_REVEAL_SETTINGS=True"
Restart=on-failure
User=www-data
Group=www-data
;; or use a file:
;EnvironmentFile=/etc/qgis-server/env

ExecStart=spawn-fcgi -s /var/run/qgisserver/qgisserver.socket -U www-data -G www-data -n /usr/lib/cgi-bin/qgis_mapserv.fcgi

[Install]
WantedBy=multi-user.target

