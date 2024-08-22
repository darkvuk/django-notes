# Deployment

The instructions in this document are based on the following tutorials, in the specified order:
1. https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu
2. https://stackoverflow.com/questions/20182329/nginx-is-throwing-an-403-forbidden-on-static-files/73175691#73175691
3. https://www.youtube.com/watch?v=dYdv6pkCufk

<hr>

Download necessary tools
<br>

```
sudo apt update
```
```
sudo apt install python3-venv python3-dev libpq-dev postgresql postgresql-contrib nginx curl
```
<br>

Set up your PostgreSQL database
```
sudo -u postgres psql
```
```
CREATE DATABASE myproject;
```
```
CREATE USER myprojectuser WITH PASSWORD 'password';
```
```
ALTER ROLE myprojectuser SET client_encoding TO 'utf8';
```
```
ALTER ROLE myprojectuser SET default_transaction_isolation TO 'read committed';
```
```
ALTER ROLE myprojectuser SET timezone TO 'UTC';
```
```
GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser;
```
```
\q
```
<br>

Set up your Git account and pull the project from your git
```
git clone https://github.com/darkvuk/dayone.git
```

Create and activate virtual environment
```
python3 -m venv .venv
```
```
source .venv/bin/activate
```
<br>

Install python packages
```
pip install django gunicorn psycopg2-binary
```
```
pip install -r requirements.txt
```
<br>

Make migrations, create superuser and collect static files
```
python manage.py makemigrations
```
```
python manage.py migrate
```
```
python manage.py createsuperuser
```
```
python manage.py collectstatic
```
<br>

Create an exception for port 8000
```
sudo ufw allow 8000
```
```
python manage.py runserver 0.0.0.0:8000
```
<br>

Now visit the link featuring your instance's IP address, instead of 0.0.0.0
```
http://35.157.111.190:8000/
```
<br>

Test Gunicorn to make sure that it can serve the application
```
gunicorn --bind 0.0.0.0:8000 myproject.wsgi
```
```
deactivate
```
<br>

Create and open a systemd socket file for Gunicorn
```
sudo nano /etc/systemd/system/gunicorn.socket
```
```
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock

[Install]
WantedBy=sockets.target
```
<br>

Create and open a systemd service file for Gunicorn
```
sudo nano /etc/systemd/system/gunicorn.service
```
```
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/dayone
ExecStart=/home/ubuntu/.venv/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/gunicorn.sock \
          dayone.wsgi:application

[Install]
WantedBy=multi-user.target
```
<br>

Start and enable the Gunicorn socket
```
sudo systemctl start gunicorn.socket
```
```
sudo systemctl enable gunicorn.socket
```
<br>

Check the status of the process to find out whether it was able to start
```
sudo systemctl status gunicorn.socket
```
<br>

Check for the existence of the gunicorn.sock file
```
file /run/gunicorn.sock
```
<br>

Check the Gunicorn socket’s logs
```
sudo journalctl -u gunicorn.socket
```
<br>

Check the status of the service
```
sudo systemctl status gunicorn
```
<br>

Test the socket activation mechanism
```
curl --unix-socket /run/gunicorn.sock localhost
```
<br>

Verify that the Gunicorn service is running
```
sudo systemctl status gunicorn
```
<br>

If a problem occurred, check the logs for additional details
```
sudo journalctl -u gunicorn
```
<br>

Reload the daemon to reread the service definition and restart the Gunicorn process
```
sudo systemctl daemon-reload
```
```
sudo systemctl restart gunicorn
```
<br>

Create and open a new server block in Nginx’s sites-available directory
```
sudo nano /etc/nginx/sites-available/myproject
```
```
server {
    listen 80;
    server_name day1.me;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/ubuntu/dayone;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/run/gunicorn.sock;
    }
}
```
<br>

Enable the file by linking it to the sites-enabled directory
```
sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled
```
<br>

Test your Nginx configuration for syntax errors
```
sudo nginx -t
```
<br>

Restart Nginx
```
sudo systemctl restart nginx
```
<br>

Open up your firewall to normal traffic on port 80. Remove the rule to open port 8000
```
sudo ufw delete allow 8000
```
```
sudo ufw allow 'Nginx Full'
```
<br>

Solve Nginx problem with static files
```
sudo gpasswd -a www-data ubuntu
```
```
nginx -s reload 
```
<br>

Reload services. Do this after each change you make.
```
sudo systemctl daemon-reload
```
```
sudo systemctl restart gunicorn
```
```
sudo systemctl restart nginx
```
<br>

Install SSL certificate
```
sudo apt install snapd
```
```
sudo snap install --classic certbot
```
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
```
sudo certbot --nginx
```
```
dev@weareai.me
```
```
y
```
```
n
```
```
1
```
