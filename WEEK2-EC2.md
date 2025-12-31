# Week 2 – EC2 Web Server

## EC2 instance details

- AMI: Ubuntu Server 24.04 LTS (64-bit, x86)
- Instance type: t3.micro (Free Tier eligible)
- Region: Europe (Stockholm) eu-north-1
- Storage: 8 GiB gp3 root volume (default)

## Security group rules

Inbound rules for the EC2 security group:

- SSH – TCP 22 – Source: 0.0.0.0/0 (allows me to SSH into the instance for admin access)
- HTTP – TCP 80 – Source: 0.0.0.0/0 (allows public web traffic to reach Nginx)

> Note: 0.0.0.0/0 means “any IP on the internet”, which is fine for this learning web server but should be restricted for production in a real environment.

## Steps I followed

1. Launched a `t3.micro` Ubuntu instance in `eu-north-1` with a new key pair (`week2-ec2-key.pem`).
2. Enabled SSH (22) and HTTP (80) in the security group so I could connect and serve web traffic.
3. Connected from my Windows laptop using SSH:

   ```bash
   ssh -i "C:\Users\munac\Desktop\week2-ec2-webserver\week2-ec2-key.pem" ubuntu@ec2-13-61-15-4.eu-north-1.compute.amazonaws.com
4. Installed and started Nginx: sudo apt update                      # refresh Ubuntu package index
sudo apt install nginx -y            # install the Nginx web server
sudo systemctl enable nginx          # start Nginx automatically on boot
sudo systemctl start nginx           # start Nginx service now

5. Replaced the default Nginx index page
cd /var/www/html
sudo rm index.html 2>/dev/null || true
printf '%s\n' '<!DOCTYPE html><html><head><title>Week 2 EC2</title></head><body><h1>Hello from EC2 (Week 2)</h1><p>Simple Nginx web server on AWS EC2.</p></body></html>' | sudo tee index.html

6. Opened the site in the browser:
Copied the EC2 Public IPv4 address from the AWS console and opened http://<public-ip>/ in my browser to see the custom page.

7. Stopped or terminated the instance
Stopped or terminated the instance afterwards to avoid ongoing compute charges.
