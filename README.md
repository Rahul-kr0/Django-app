# Django-app
Deployed a Django application with nginx, docker and backend with sqlite.

For Testing Environment:

Step 1: Update the Package List

$ sudo apt update

This command updates the list of available packages on your Ubuntu system.

Step 2: Create a New User

$ sudo adduser new_username

This command creates a new user named new_username . You'll be prompted to set a

password and provide user information.

Step 3: Grant Sudo Privileges

$ sudo usermod -aG sudo new_username

This command adds the user new_username to the sudo group, granting them sudo

(administrative) privileges.

Step 4: Switch to the Django App User

$ sudo su - django-app

This command switches the current user to the django-app user.

Step 5: Install Nginx

$ sudo apt install nginx

This command installs the Nginx web server.

Step 6: Check Nginx Status

$ systemctl status nginx

This command checks the status of the Nginx service to ensure it's running.

Step 7: Clone Your Django App Repository

$ git clone <https://github.com/LondheShubham153/django-notes-app.git>

$ cd django-notes-app

These commands clone your Django app's repository from GitHub and navigate to its

directory.

Step 8: Install Docker

$ sudo apt install docker.io

This command installs Docker, a containerization platform, on your server.

Step 9: Add User to Docker Group

$ sudo usermod -aG docker $USER

This command adds the current user to the Docker group, allowing you to run Docker

commands without using sudo .

Step 10: Check Docker Containers

$ sudo docker ps

This command checks and lists the running Docker containers on your server.

Step 11: Build the Docker Image

$ cd django-notes-app

$ docker build -t notes-app .

These commands navigate to your Django app's directory and build a Docker image

named notes-app from the current directory ( . ).

Step 12: Run the Docker Container

$ docker run -d -p 8000:8000 notes-app:latest

This command runs a Docker container based on the notes-app:latest image, exposing

port 8000.

Step 13: Test Your Django App

$ curl -L <http://127.0.0.1:8000>

This command tests your Django app by making an HTTP request to

$ http://127.0.0.1:8000 .

Step 14: Configure Nginx Default Server Block

$ cd /etc/nginx/sites-enabled

$ sudo vim default

These commands open the default Nginx server block configuration for editing using the

$ Vim text editor.

Step 15: Nginx Server Block Configuration

Inside the Vim editor, make the following configuration changes:

server {

listen 80 default_server;

listen [::]:80 default_server;

root /var/www/html;

index index.html index.htm index.nginx-debian.html;

server_name _;

location / {

proxy_pass <http://127.0.0.1:8000>;

try_files $uri $uri/ =404;

}

}

Replace <http://127.0.0.1:8000> with http://localhost:8000 in the proxy_pass directive.

Step 16: Copy Django App Files to Nginx's Document Root

$ cd build

$ sudo cp -r * /var/www/html/

These commands navigate to the build directory of your Django app and copy its files

to Nginx's document root ( /var/www/html/ ).

Step 17: Test Your Django App via Nginx

$ curl -L <http://127.0.0.1:8000/api>

This command tests your Django app through Nginx by making an HTTP request to

$ http://127.0.0.1:8000/api .

Step 18: Configure Nginx for API Endpoints (Optional)

You've already provided a configuration for the API endpoint. Ensure that you save the

configuration and exit the Vim editor.

For production-ready environment:

Step 1: Secure the Server

1.1. Update the system packages:

$ sudo apt update

$ sudo apt upgrade -y

1.2. Configure a firewall (e.g., UFW) to allow only necessary traffic. For example, allow

SSH, HTTP, and HTTPS:

$ sudo ufw allow OpenSSH

$ sudo ufw allow 'Nginx Full'

$ sudo ufw enable

1.3. Set up automatic security updates:

$ sudo apt install unattended-upgrades -y

$ Edit the /etc/apt/apt.conf.d/20auto-upgrades file to enable automatic updates:

$ sudo nano /etc/apt/apt.conf.d/20auto-upgrades

Ensure it looks like this:

APT::Periodic::Update-Package-Lists "1";

APT::Periodic::Download-Upgradeable-Packages "1";

APT::Periodic::AutocleanInterval "7";

APT::Periodic::Unattended-Upgrade "1";

1.4. Implement server monitoring and logging solutions for security and performance

monitoring.

Step 2: Set Up SSL/TLS (HTTPS)

2.1. Obtain a free Let's Encrypt SSL/TLS certificate for your domain:

$ sudo apt install certbot python3-certbot-nginx -y

$ sudo certbot --nginx -d your_domain.com -d www.your_domain.com

2.2. Follow the prompts to configure SSL/TLS for your app.

Step 3: Optimize Nginx for Production

3.1. Tune Nginx configuration for better performance. Edit the Nginx configuration:

$ sudo nano /etc/nginx/nginx.conf

Adjust the worker_processes and worker_connections values based on your server's

resources. For example:

worker_processes auto;
events {
worker_connections 1024;

}

3.2. Enable Nginx Gzip compression for better page load times:

gzip on;

gzip_types text/plain text/css application/json application/javascript text/xml applicatio

n/xml application/xml+rss text/javascript;

gzip_comp_level 6;

gzip_min_length 1000;

3.3. Implement Nginx caching to improve response times:

location / {

proxy_pass <http://localhost:8000>;

proxy_set_header Host $host;

proxy_set_header X-Real-IP $remote_addr;

# Add cache settings here

}

You can configure caching based on your app's requirements.

Step 4: Set Up Database Backups

Since your app uses SQLite, you can create regular backups using a cron job. Install

sqlite3 if it's not already installed:

sudo apt install sqlite3 -y

Create a shell script to perform the backup:

nano /path/to/backup_script.sh

Add the following content, replacing /path/to/your/db.sqlite3 with the actual path to your

SQLite database:

#!/bin/bash

backup_dir="/path/to/backup/directory"

timestamp=$(date +"%Y%m%d%H%M%S")

backup_file="$backup_dir/db_backup_$timestamp.sqlite3"

sqlite3 /path/to/your/db.sqlite3 .backup $backup_file

Make the script executable:

chmod +x /path/to/backup_script.sh

Create a cron job to run the backup script regularly. Open the crontab editor:

crontab -e


Add a line to run the script daily at midnight, for example:

0 0 * * * /path/to/backup_script.sh

Step 5: Monitor Your Production Server

Set up server monitoring and alerting using tools like Prometheus, Grafana, or other

monitoring solutions. Monitor resource usage, app performance, and server health.

Step 6: Write Documentation/Instructions

By following these additional steps, your Django app will be deployed in a production-

ready environment with enhanced security, performance, and robustness. Keep your

server and app up to date, regularly back up your data, and monitor your production

environment to ensure its reliability and availability.
