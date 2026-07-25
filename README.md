# 🚀 Apache + HTTPS Setup on AWS EC2 (mylawyersai.com)

This guide explains how to deploy **mylawyersai.com** on an AWS EC2 instance using **Apache2** and secure it with a free **Let's Encrypt SSL certificate**.

---

# 📋 Prerequisites

Before starting, make sure you have:

- AWS EC2 Ubuntu Server
- A registered domain (`mylawyersai.com`)
- Route 53 Hosted Zone
- Hostinger Domain DNS Management
- SSH access to your EC2 instance

---

# Step 1: Configure Name Servers

Go to your **AWS Route 53 Hosted Zone**.

Copy all **4 AWS Name Servers**.

Example:

```
ns-123.awsdns-45.com
ns-456.awsdns-78.net
ns-789.awsdns-12.org
ns-321.awsdns-65.co.uk
```

Now login to **Hostinger**.

Navigate to:

```
Domains
    ↓
DNS / Nameservers
    ↓
Change Nameservers
```

Replace the existing Hostinger nameservers with the **4 AWS Route 53 nameservers**.

Wait for DNS propagation (usually 5–30 minutes, sometimes up to 24 hours).

---

# Step 2: Connect to AWS EC2

SSH into your server.

```bash
ssh -i your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

Update the package list.

```bash
sudo apt update
```

---

# Step 3: Install Apache2

Install Apache and Certbot.

```bash
sudo apt install apache2 certbot python3-certbot-apache -y
```

---

# Step 4: Enable Apache

Enable Apache to start automatically.

```bash
sudo systemctl enable apache2
```

Start Apache.

```bash
sudo systemctl start apache2
```

---

# Step 5: Verify Apache Status

Check whether Apache is running.

```bash
sudo systemctl status apache2
```

Expected output:

```
Active: active (running)
```

Exit the status screen by pressing:

```
Q
```

---

# Step 6: Allow HTTP & HTTPS

If UFW Firewall is enabled:

```bash
sudo ufw allow 'Apache Full'
sudo ufw reload
```

For AWS EC2 Security Group, ensure the following ports are open:

| Port | Protocol | Purpose |
|-------|----------|----------|
| 22 | TCP | SSH |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |

---

# Step 7: Install SSL Certificate

Generate a free SSL certificate using Let's Encrypt.

```bash
sudo certbot --apache -d mylawyersai.com -d www.mylawyersai.com
```

Follow the prompts:

- Enter your email
- Accept Terms of Service
- Choose whether to redirect HTTP → HTTPS

Select:

```
Redirect HTTP to HTTPS
```

---

# Step 8: Verify HTTPS

Open your website.

```
https://mylawyersai.com
```

or

```
https://www.mylawyersai.com
```

A secure 🔒 lock icon should appear in the browser.

---

# Step 9: Verify SSL Renewal

Let's Encrypt certificates expire every 90 days.

Test automatic renewal.

```bash
sudo certbot renew --dry-run
```

If successful, automatic renewal is configured correctly.

---

# Useful Commands

Update packages

```bash
sudo apt update
```

Restart Apache

```bash
sudo systemctl restart apache2
```

Reload Apache

```bash
sudo systemctl reload apache2
```

Stop Apache

```bash
sudo systemctl stop apache2
```

Start Apache

```bash
sudo systemctl start apache2
```

Apache Status

```bash
sudo systemctl status apache2
```

Check Apache Configuration

```bash
sudo apache2ctl configtest
```

---

# Troubleshooting

## SSL Certificate Failed

Verify DNS:

```bash
nslookup mylawyersai.com
```

or

```bash
dig mylawyersai.com
```

The domain should point to your EC2 Public IP.

---

## Apache Not Running

Restart Apache.

```bash
sudo systemctl restart apache2
```

View logs.

```bash
sudo journalctl -u apache2
```

---

## Port 80 or 443 Not Accessible

Check Security Groups.

Allow:

- TCP 80
- TCP 443

---

# Project Stack

- AWS EC2
- Ubuntu
- Apache2
- Route 53
- Hostinger DNS
- Let's Encrypt
- Certbot

---

# Author

**Avinash Arya**

**Project:** MyLawyersAI

Website:

https://mylawyersai.com

---

⭐ If this guide helped you, consider giving the repository a **Star**.
