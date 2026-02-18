# 🚨 SSH Login Alert Automation

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Linux](https://img.shields.io/badge/Platform-Linux-green)
![SSH](https://img.shields.io/badge/Trigger-SSH_Login-orange)
![Status](https://img.shields.io/badge/Status-Production_Ready-brightgreen)

Automatically send email alerts whenever a user logs in to your server via SSH.

This lightweight Python script:
- ✅ Detects SSH logins
- 📧 Sends email alerts
- 📝 Logs login activity
- 🛡 Supports IP whitelisting
- 🔒 Uses secure environment variables for credentials

---

## 📂 Project Structure

| File | Description |
|------|------------|
| `/usr/local/bin/ssh_login_alert.py` | Main alert script |
| `/var/log/ssh_login_alert.log` | Log file |

---

## 🚀 Setup Guide

---

## 1️⃣ Create the Script

```bash
sudo vim /usr/local/bin/ssh_login_alert.py
```

Paste the script content.

---

## 2️⃣ Update Script Configuration (Required)

Open the script and update:

### 🔹 Server Name
```python
SERVER_NAME = "PROD-SERVER"
```

---

### 🔹 Sender Email (EMAIL_FROM)
```python
EMAIL_FROM = "your_email@gmail.com"
```

---

### 🔹 Receiver Emails (EMAIL_TO)
```python
EMAIL_TO = [
    "admin@example.com",
    "security@example.com"
]
```

---

### 🔹 Whitelisted IP Addresses
```python
WHITELIST_IPS = [
    "10.118.7.46",
]
```

Whitelisted IPs:
- Will be logged
- Will NOT trigger email alerts

---

## 3️⃣ Set Required Permissions

```bash
sudo chmod +x /usr/local/bin/ssh_login_alert.py

sudo touch /var/log/ssh_login_alert.log
sudo chown root:adm /var/log/ssh_login_alert.log
sudo chmod 664 /var/log/ssh_login_alert.log
sudo chmod 1777 /var/log/ssh_login_alert.log
```

---

## 4️⃣ Configure SSH to Trigger Script

Edit SSH RC file:

```bash
sudo vim /etc/ssh/sshrc
```

Add:

```bash
/usr/bin/env python3 /usr/local/bin/ssh_login_alert.py
```

Save and exit.

---

## 5️⃣ Configure Email Password (Secure Method)

⚠️ Use **Gmail App Password** (NOT your main Gmail password).

Edit:

```bash
sudo vim /etc/profile
```

Add:

```bash
export ALERT_EMAIL_PASS="your_gmail_app_password"
```

Reload:

```bash
source /etc/profile
```

---

## 6️⃣ Restart SSH Service

```bash
sudo systemctl restart ssh
```

---

# 🧪 Testing

1. Open a new SSH session.
2. Check logs:

```bash
cat /var/log/ssh_login_alert.log
```

3. Confirm email is received.

---

# 📜 Example Alert Output

```
============================================
 SSH LOGIN DETECTED ON PROD-SERVER
User             : ubuntu
Login IP         : 14.xx.xx.xx
Server Public IP : 3.xx.xx.xx
Server Hostname  : ip-172-31-xx-xx
Date/Time        : 2026-02-18 10:45:12
============================================
```

---

# 🛠 Troubleshooting

### Email not sending?

Check:

```bash
echo $ALERT_EMAIL_PASS
```

If empty:

```bash
source /etc/profile
```

Make sure:
- Gmail 2-Step Verification is enabled
- App Password is used
- SMTP port 587 is allowed

---

# 🔐 Security Best Practices

- ❌ Never hardcode passwords
- ✅ Use Gmail App Password
- ✅ Restrict file permissions
- ✅ Keep whitelist minimal
- ✅ Monitor logs regularly

---

# 📌 Features Summary

| Feature | Status |
|----------|--------|
| SSH Login Detection | ✅ |
| Email Alerts | ✅ |
| IP Whitelisting | ✅ |
| Public IP Detection | ✅ |
| Logging | ✅ |
| Production Ready | ✅ |

---

# 🎯 Final Result

After setup:

- Every SSH login triggers the script
- Email alert is sent
- Login activity is logged
- Whitelisted IPs are ignored

---

## 👨‍💻 Author

Rajat Kumar  
DevOps Engineer  

---

⭐ If this project helped you, consider starring the repository.
