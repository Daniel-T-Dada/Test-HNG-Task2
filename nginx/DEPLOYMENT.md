# Deployment Guide

This guide explains how to deploy the FastAPI Book Management API using Nginx as a reverse proxy.

## Prerequisites

- Ubuntu 20.04 or later
- Python 3.12
- Nginx
- Systemd

## Installation Steps

1. Install Required Packages:

```bash
sudo apt update
sudo apt install python3.12 python3.12-venv nginx
```

2. Clone the Repository:

```bash
git clone https://github.com/your-username/fastapi-book-project.git
cd fastapi-book-project
```

3. Set Up Python Environment:

```bash
python3.12 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

4. Configure Nginx:

```bash
# Copy Nginx configuration
sudo cp nginx/fastapi_book.conf /etc/nginx/sites-available/
sudo ln -s /etc/nginx/sites-available/fastapi_book.conf /etc/nginx/sites-enabled/

# Test and restart Nginx
sudo nginx -t
sudo systemctl restart nginx
```

5. Set Up SystemD Service:

```bash
# Copy and modify service file
sudo cp nginx/fastapi_book.service /etc/systemd/system/
sudo nano /etc/systemd/system/fastapi_book.service  

# Start service
sudo systemctl daemon-reload
sudo systemctl start fastapi_book
sudo systemctl enable fastapi_book
```

6. Check Status:

```bash
sudo systemctl status fastapi_book
sudo nginx -t
curl http://localhost/healthcheck
```

## SSL Configuration (Optional)

1. Install Certbot:

```bash
sudo apt install certbot python3-certbot-nginx
```

2. Obtain SSL Certificate:

```bash
sudo certbot --nginx -d your-domain.com
```

## Troubleshooting

1. Check Nginx Logs:

```bash
sudo tail -f /var/log/nginx/error.log
sudo tail -f /var/log/nginx/access.log
```

2. Check Application Logs:

```bash
sudo journalctl -u fastapi_book -f
```

3. Common Issues:

- Permission denied: Check file permissions and ownership
- Port in use: Ensure no other service is using port 80/443
- 502 Bad Gateway: Check if FastAPI application is running

## Maintenance

1. Update Application:

```bash
git pull
source venv/bin/activate
pip install -r requirements.txt
sudo systemctl restart fastapi_book
```

2. Nginx Commands:

```bash
sudo systemctl stop nginx    # Stop Nginx
sudo systemctl start nginx   # Start Nginx
sudo systemctl restart nginx # Restart Nginx
```

3. Application Commands:

```bash
sudo systemctl stop fastapi_book    # Stop application
sudo systemctl start fastapi_book   # Start application
sudo systemctl restart fastapi_book # Restart application
```
