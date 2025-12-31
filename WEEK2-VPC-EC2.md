# Week 2 – VPC & EC2 Setup

## VPC details

- VPC CIDR: 10.0.0.0/16 (private address space for my lab network in AWS).[web:266][web:267]
- Public subnet: 10.0.1.0/24 (created inside the VPC and configured to auto-assign public IPv4 addresses).[web:266]
- Internet Gateway (IGW) attached to the VPC.
- Route table updated with a route `0.0.0.0/0 -> IGW` for the public subnet so instances can reach the internet.[web:266][web:267]

## Security group rules

Security group created in the custom VPC with stricter inbound rules:

- SSH – TCP 22 – Source: `<my-ip>/32` (only my current IP can SSH in, instead of 0.0.0.0/0).[web:265][web:260]
- HTTP – TCP 80 – Source: 0.0.0.0/0 (allows public web traffic if I run a web server on the instance).[web:265]
- Outbound – all traffic allowed (default).[web:265]

## EC2 instance details

- AMI: Ubuntu Server 24.04 LTS (64-bit, x86).[web:183]
- Instance type: t3.micro (Free Tier eligible).
- Network: launched in the **custom VPC**, **public subnet**, and attached the **strict security group** above.[web:266]
- Auto-assign public IPv4 address: enabled so the instance gets a public IP.
- Key pair: `week2-vpc-ec2-key.pem` to allow SSH access from my laptop.[web:274]

## Steps I followed

1. Created a custom VPC with CIDR `10.0.0.0/16` in the VPC console to use as an isolated network for this project.[web:266][web:267]
2. Created a public subnet `10.0.1.0/24` in that VPC and enabled auto-assign public IPv4 addresses so instances launched there get a public IP by default.[web:266]
3. Created an Internet Gateway (IGW), attached it to the VPC, and updated the public subnet's route table with a `0.0.0.0/0 -> IGW` route so instances can reach the internet.[web:266][web:267]
4. Created a security group in the same VPC with:
   - Inbound SSH (TCP 22) allowed **only** from my IP (`<my-ip>/32`) to keep admin access locked down.
   - Inbound HTTP (TCP 80) allowed from `0.0.0.0/0` so a future web server would be publicly reachable.
   - Outbound traffic left as default (allow all).[web:265][web:260]

5. Launched a `t3.micro` Ubuntu 24.04 instance in the custom VPC and public subnet, attached the strict security group, and selected the `week2-vpc-ec2-key.pem` key pair.[web:183][web:264]
6. After the instance was running, copied its public IPv4 address from the EC2 console and connected from my Windows laptop using SSH:

   ```bash
   ssh -i "C:\path\to\week2-vpc-ec2-key.pem" ubuntu@<public-ip>
   ```[web:274]

7. Verified connectivity on the instance (for example, by running `hostname` and `curl` from the terminal) and confirmed that the instance could reach the internet through the IGW and route table.[web:266]
8. (Optional) Installed Nginx again using `sudo apt update` and `sudo apt install nginx -y` to test HTTP access via the strict security group.[web:233][web:200]
9. Stopped or terminated the instance after testing to avoid ongoing compute charges.[web:255]
