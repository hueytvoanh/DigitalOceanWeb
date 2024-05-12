# DigitalOceanWeb

Load DATA 
rm -rf /var/log/nginx/*
nano /etc/nginx/sites-available/sunnyiot.org

DELETE-RESTART
     nginx -t
     systemctl restart emperor.uwsgi.service
     systemctl restart nginx

4. Install Node RED
https://randomnerdtutorials.com/access-node-red-dashboard-anywhere-digital-ocean/
Install Palette
     node-red-contrib-influxdb

Create PYTHON environment 



mkdir /var/www/lab_app/
cd /var/www/lab_app/
/usr/local/opt/python-3.8.2/bin/python3.8 -m venv .
 . bin/activate
  pip install flask
  pip install --upgrade pip


  bin/uwsgi --ini /var/www/lab_app/lab_app_uwsgi.ini

  nano /etc/systemd/system/emperor.uwsgi.service

 systemctl daemon-reload
systemctl start emperor.uwsgi.service
 systemctl enable emperor.uwsgi.service


 InfluxDB
 sudo apt install influxdb
 sudo systemctl unmask influxdb
 sudo systemctl enable influxdb
 sudo systemctl start influxdb
 apt install influxdb-client
 influx

 sudo systemctl restart influxdb
 influx -username admin -password 159381
 CREATE DATABASE sensors
 
 
 Grafana
 rm -rf /etc/apt/sources.list.d/*grafana
 
 sudo rm -i /etc/apt/sources.list.d/grafana.list
 /etc/apt/sources.list.d/archive_uri-https_packages_grafana_com_oss_deb-mantic.list:1 
 /etc/apt/sources.list.d/grafana.list
 https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-grafana-on-ubuntu-22-04
 
 ###https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-grafana-on-ubuntu-18-04
 ###[snap install grafana](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/)

 sudo systemctl start grafana-server
sudo systemctl enable grafana-server
netstat -lptn

To access from anywhre 
sudo ufw allow 3000
IP:3000


>>> from project import app, db
>>> app.app_context().push()
>>> db.create_all()

USER DATABASE
    sqlite3 database.db
    .tables
    select * from user;
    insert into user values (NULL, "ats1", "159123");

DATABASE
sqlite3 acu_ats.db
create table temperatures (rDatetime datetime, sensorID text, userID, temp numeric);
create table temperatures (rDatetime datetime, sensorID nummeric, temp numeric, userID text);
insert into temperatures values (datetime(CURRENT_TIMESTAMP),"1", "admin", 25.7);


SELECT * FROM temperatures WHERE sensorID=1 AND userID="admin" AND (rDatetime BETWEEN '2024-05-01 00:00' AND '2024-05-01 14:21);

SELECT * FROM temperatures WHERE sensorID=1 AND userID="admin" AND (rDatetime BETWEEN '2024-05-01 00:00' AND '2024-05-01 14:21');
SELECT * FROM temperatures WHERE rDatetime BETWEEN date('2024-05-01') AND date('2024-05-02');
%s/\t/  /g


sqlite3 acu_ats_2.db
create table temperatures (rDatetime datetime, sensorID nummeric, temp numeric, userID text);
create table currents (rDatetime datetime, sensorID nummeric, current numeric, userID text);
create table voltages (rDatetime datetime, sensorID nummeric, voltage numeric, userID text);


DATE
date
timedatectl set-timezone Asia/Ho_Chi_Minh


insert into temperatures values (datetime(CURRENT_TIMESTAMP), 1, 25.7, "admin");
select * from currents;
select * from voltages;
select * from tempratures;
