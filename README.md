# DigitalOceanWeb

# Install Nginx
    sudo apt update \n
    sudo apt install nginx \n

# Set Up firewall
    sudo apt-get install ufw \n
    sudo ufw app list \n
    sudo ufw enable \n
    sudo ufw allow 'Nginx HTTP' \n
    sudo ufw allow 22 \n
    sudo ufw allow 80 \n
    sudo ufw allow 443 \n
    sudo ufw allow 1883 \n
    sudo ufw allow 9001 \n

    Check nginx work 192.168.1.7 \n

# HTML-NGINX
    sudo mkdir -p /var/www/sunnyiot.duckdns.org/html \n
    sudo chown -R $USER:$USER /var/www/sunnyiot.duckdns.org/html \n
    sudo chmod -R 755 /var/www/sunnyiot.duckdns.org \n
    sudo nano /var/www/sunnyiot.duckdns.org/html/index.html \n
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

sudo ln -s /etc/nginx/sites-available/sunnyiot.duckdns.org /etc/nginx/sites-enabled/ \n

sudo nano /etc/nginx/nginx.conf \n

server_names_hash_bucket_size 64; \n


# SET UP DUCKDNS \n
sudo mkdir duckdns \n
cd ~/duckdns/ \n
sudo nano duckdns.sh \n 
echo url="https://www.duckdns.org/update?domains=sunnyiot&token=0fcfb91c-e7c8-4bf7-9d2d-551c3cfa0f50&ip=" | curl -k -o ~/duckdns/duck.log -K - \n
sudo chmod 700 duckdns.sh \n
sudo crontab -e \n
     */5 * * * * ~/duckdns/duckdns.sh >/dev/null 2>&1 \n

sudo chmod 777 ./duckdns.sh \n
sudo chmod 777 ~/duckdns/*    \n 

sudo apt install certbot python3-certbot-nginx \n
sudo nano /etc/nginx/sites-available/sunnyiot.duckdns.org \n

sudo ufw allow 'Nginx Full' \n
sudo ufw delete allow 'Nginx HTTP' \n

# OBTAIN SSL 
sudo certbot --nginx -d sunnyiot.duckdns.org -d www.sunnyiot.duckdns.org \n
Get cert files in: /etc/letencrypt/live/sunnyiot.duckdns.org/ \n

sudo systemctl status certbot.timer \n
sudo certbot renew --dry-run \n

CHECKING DOMAIN NAME IS WORKING \n
https://sunnyiot.duckdns.org/ \n
Success! The your_domain server block is working! \n

# SET UP APP
sudo nano /etc/dphys-swapfile \n
change configurationnumber to 1024 \n

 $ sudo apt-get update \n
 $ sudo apt-get upgrade \n
 $ sudo apt-get install build-essential \n
 $ sudo apt-get install libncurses5-dev libncursesw5-dev libreadline6-dev libffi-dev \n
 $ sudo apt-get install libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev libsqlite3-dev libgdbm-dev libssl-dev openssl \n
 $ sudo apt-get install libboost-python-dev \n
 $ sudo apt-get install libpulse-dev \n
 $ sudo apt-get install python-dev \n
 #### $ sudo apt-get install python-dev-is-python3 \n
 $ sudo apt-get install vim \n

 $ cd ~ \n
 $ mkdir python-source \n
 $ cd python-source/  \n
 $ wget https://www.python.org/ftp/python/3.8.2/Python-3.8.2.tgz \n
 $ tar zxvf Python-3.8.2.tgz \n
 $ cd Python-3.8.2/ \n
 $ ./configure --prefix=/usr/local/opt/python-3.8.2 \n
 $ make \n
 $ sudo make install \n
 $ /usr/local/opt/python-3.8.2/bin/python3.8 --version \n

 $ sudo su \n
 # mkdir /var/www \n
 # mkdir /var/www/lab_app/ \n
 # cd /var/www/lab_app/ \n
 # /usr/local/opt/python-3.8.2/bin/python3.8 -m venv . \n
 # . bin/activate \n


 https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uswgi-and-nginx-on-ubuntu-18-04 \n
 (lab_app) root@raspberrypi-zero:/var/www/lab_app# pip install flask \n
 nano hello.py \n

 ------------------------------------------------------------------------------
  from flask import Flask
 app = Flask(__name__)
 @app.route("/")
 def hello():
    return "Hello World!"
 if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
--------------------------------------------------------------------------------
(lab_app) sudo ufw allow 5000
CHECK FLASK WORK 
python hello.py
http://192.168.1.7:5000/              =============> Hello

(lab_app) root@raspberrypi-zero:/var/www/lab_app# pip install uwsgi

nano lab_app_uwsgi.ini
----------------------------------------------------------------------------------
[uwsgi]
#application's base folder
base = /var/www/lab_app
#python module to import
app = lab_app_v9
module = %(app)
home = %(base)
pythonpath = %(base)
#socket file's location
socket = /var/www/lab_app/%n.sock
#permissions for the socket file
chmod-socket = 666
#the variable that holds a flask
#application inside the module
#imported at line #6
callable = app
#location of log files
logto = /var/log/uwsgi/%n.log
----------------------------------------------------------------------------------------

cd /etc/systemd/system/
nano emperor.uwsgi.service
-----------------------------------------------------------------------------------------
Unit]
Description=uWSGI Emperor
After=syslog.target

# [Service]
ExecStart=/var/www/lab_app/bin/uwsgi --ini /var/www/lab_app/lab_app_uwsgi.ini
# Requires systemd version 211 or newer
RuntimeDirectory=uwsgi
Restart=always
KillSignal=SIGQUIT
Type=notify
StandardError=syslog
NotifyAccess=all
[Install]
WantedBy=multi-user.target
-------------------------------------------------------------------------------------------

systemctl start emperor.uwsgi.service \n
systemctl status emperor.uwsgi.service \n
systemctl enable emperor.uwsgi.service \n



# Load DATA 
rm -rf /var/log/nginx/* \n
nano /etc/nginx/sites-available/sunnyiot.org \n

# DELETE-RESTART
     nginx -t
     systemctl restart emperor.uwsgi.service
     systemctl restart nginx

# 4. Install Node RED
https://randomnerdtutorials.com/access-node-red-dashboard-anywhere-digital-ocean/ \n
Install Palette \n
     node-red-contrib-influxdb \n

Create PYTHON environment  \n



mkdir /var/www/lab_app/ \n
cd /var/www/lab_app/ \n
/usr/local/opt/python-3.8.2/bin/python3.8 -m venv . \n
 . bin/activate \n
  pip install flask \n
  pip install --upgrade pip \n


  bin/uwsgi --ini /var/www/lab_app/lab_app_uwsgi.ini \n
  nano /etc/systemd/system/emperor.uwsgi.service \n

 systemctl daemon-reload \n
 systemctl start emperor.uwsgi.service \n
 systemctl enable emperor.uwsgi.service \n


 # InfluxDB
 sudo apt install influxdb \n
 sudo systemctl unmask influxdb \n
 sudo systemctl enable influxdb \n
 sudo systemctl start influxdb \n
 apt install influxdb-client \n
 influx \n

 sudo systemctl restart influxdb \n
 influx -username admin -password 159381 \n
 CREATE DATABASE sensors \n
 
 
 # Grafana
 rm -rf /etc/apt/sources.list.d/*grafana \n
 
 sudo rm -i /etc/apt/sources.list.d/grafana.list \n
 /etc/apt/sources.list.d/archive_uri-https_packages_grafana_com_oss_deb-mantic.list:1  \n
 /etc/apt/sources.list.d/grafana.list \n
 https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-grafana-on-ubuntu-22-04 \n
 
 ###https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-grafana-on-ubuntu-18-04 \n
 ###[snap install grafana](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/) \n

sudo systemctl start grafana-server \n
sudo systemctl enable grafana-server \n
netstat -lptn \n

To access from anywhre  \n
sudo ufw allow 3000 \n
IP:3000 \n


>>> from project import app, db
>>> app.app_context().push()
>>> db.create_all()

# USER DATABASE
    sqlite3 database.db
    .tables
    select * from user;
    insert into user values (NULL, "ats1", "159123");    
    insert into user values (NULL, "ats2", "15912345");      
    insert into user values (NULL, "ats3", "15912355");

# DATABASE
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
