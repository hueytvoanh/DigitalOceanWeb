# DigitalOceanWeb

#Install Nginx
    sudo apt update
    sudo apt install nginx

Set Up firewall
    sudo apt-get install ufw
    sudo ufw app list
    sudo ufw enable
    sudo ufw allow 'Nginx HTTP'
    sudo ufw allow 22
    sudo ufw allow 80
    sudo ufw allow 443
    sudo ufw allow 1883
    sudo ufw allow 9001

    Check nginx work 192.168.1.7

HTML-NGINX
    sudo mkdir -p /var/www/sunnyiot.duckdns.org/html
    sudo chown -R $USER:$USER /var/www/sunnyiot.duckdns.org/html
    sudo chmod -R 755 /var/www/sunnyiot.duckdns.org
    sudo nano /var/www/sunnyiot.duckdns.org/html/index.html
---------------------------------------------------------------------------------------------------------------------------
    <html>
    <head>
        <title>Welcome to your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain server block is working!</h1>
    </body>
    </html>
---------------------------------------------------------------------------------------------------------------------------

sudo nano /etc/nginx/sites-available/sunnyiot.duckdns.org

--------------------------------------------------------------------------------------------------------------------------
server {
        listen 80;
        listen [::]:80;

        root /var/www/sunnyiot.duckdns.org/html;
        index index.html index.htm index.nginx-debian.html;

        server_name sunnyiot.duckdns.org www.sunnyiot.duckdns.org;

        location / {
                try_files $uri $uri/ =404;
        }
}

---------------------------------------------------------------------------------------------------------------------------

sudo ln -s /etc/nginx/sites-available/sunnyiot.duckdns.org /etc/nginx/sites-enabled/

sudo nano /etc/nginx/nginx.conf

server_names_hash_bucket_size 64;


#SET UP DUCKDNS
sudo mkdir duckdns
cd ~/duckdns/
sudo nano duckdns.sh 
echo url="https://www.duckdns.org/update?domains=sunnyiot&token=0fcfb91c-e7c8-4bf7-9d2d-551c3cfa0f50&ip=" | curl -k -o ~/duckdns/duck.log -K -
sudo chmod 700 duckdns.sh
sudo crontab -e
     */5 * * * * ~/duckdns/duckdns.sh >/dev/null 2>&1

sudo chmod 777 ./duckdns.sh
sudo chmod 777 ~/duckdns/*     

sudo apt install certbot python3-certbot-nginx
sudo nano /etc/nginx/sites-available/sunnyiot.duckdns.org

sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'

OBTAIN SSL
sudo certbot --nginx -d sunnyiot.duckdns.org -d www.sunnyiot.duckdns.org
Get cert files in: /etc/letencrypt/live/sunnyiot.duckdns.org/

sudo systemctl status certbot.timer
sudo certbot renew --dry-run

CHECKING DOMAIN NAME IS WORKING
https://sunnyiot.duckdns.org/
Success! The your_domain server block is working!

SET UP APP
sudo nano /etc/dphys-swapfile
change configurationnumber to 1024

 $ sudo apt-get update
 $ sudo apt-get upgrade
 $ sudo apt-get install build-essential
 $ sudo apt-get install libncurses5-dev libncursesw5-dev libreadline6-dev libffi-dev
 $ sudo apt-get install libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev libsqlite3-dev libgdbm-dev libssl-dev openssl
 $ sudo apt-get install libboost-python-dev
 $ sudo apt-get install libpulse-dev
 $ sudo apt-get install python-dev
 #### $ sudo apt-get install python-dev-is-python3
 $ sudo apt-get install vim

 $ cd ~
 $ mkdir python-source
 $ cd python-source/
 $ wget https://www.python.org/ftp/python/3.8.2/Python-3.8.2.tgz
 $ tar zxvf Python-3.8.2.tgz
 $ cd Python-3.8.2/
 $ ./configure --prefix=/usr/local/opt/python-3.8.2
 $ make
 $ sudo make install
 $ /usr/local/opt/python-3.8.2/bin/python3.8 --version

 $ sudo su
 # mkdir /var/www
 # mkdir /var/www/lab_app/
 # cd /var/www/lab_app/
 # /usr/local/opt/python-3.8.2/bin/python3.8 -m venv .
 # . bin/activate
 
 (lab_app) root@raspberrypi-zero:/var/www/lab_app# pip install flask
 nano hello.py

 ------------------------------------------------------------------------------
  from flask import Flask
 app = Flask(__name__)
 @app.route("/")
 def hello():
    return "Hello World!"
 if __name__ == "__main__":
    app.run(host='0.0.0.0', port=8080)
--------------------------------------------------------------------------------


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
    insert into user values (NULL, "ats2", "15912345");      
    insert into user values (NULL, "ats3", "15912355");

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
