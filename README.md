# SSH Login Alert Automation

This project sets up an automatic email alert whenever any user logs in to the server via SSH.

The script:
- Triggers automatically on SSH login
- Logs login details to a file
- Sends email alerts
- Ignores whitelisted IP addresses (e.g., Jenkins server)

---

## 📌 File Location

Script Path:
```
/usr/local/bin/ssh_login_alert.py
```

Log File:
```
/var/log/ssh_login_alert.log
```

---

## ⚙️ Setup Instructions

### 1️⃣ Create the Script

```bash
sudo vim /usr/local/bin/ssh_login_alert.py
```

Paste the script content.

Update this variable inside the script:

```python
SERVER_NAME = "<enter-the-server-name>"
```

Replace it with your actual server name.

---

### 2️⃣ Set Required Permissions

```bash
sudo chmod +x /usr/local/bin/ssh_login_alert.py

sudo touch /var/log/ssh_login_alert.log
sudo chown root:adm /var/log/ssh_login_alert.log
sudo chmod 664 /var/log/ssh_login_alert.log
sudo chmod 1777 /var/log/ssh_login_alert.log
```

---

### 3️⃣ Configure SSH to Trigger Script Automatically

Edit SSH RC file:

```bash
sudo vim /etc/ssh/sshrc
```

Add this line at the bottom:

```bash
/usr/bin/env python3 /usr/local/bin/ssh_login_alert.py
```

Save and exit.

---

### 4️⃣ Configure Email Password (Environment Variable)

Edit system profile:

```bash
sudo vim /etc/profile
```

Add:

```bash
export ALERT_EMAIL_PASS="<enter_your_mail_password>"
```

Reload environment:

```bash
source /etc/profile
```

---

### 5️⃣ Restart SSH Service

```bash
sudo systemctl restart ssh
```

---

## 🧪 Testing the Setup

1. Open a new SSH session.
2. Check log file:

```bash
cat /var/log/ssh_login_alert.log
```

3. Verify email alert is received.

---

## 🛡 Whitelisting IPs

Inside the script:

```python
WHITELIST_IPS = [
    "10.118.7.46",
]
```

Add any IP address that should NOT trigger email alerts.

If a whitelisted IP logs in:
- It will be logged
- Email will NOT be sent

---

## 📝 Log Output Example

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

## 🚨 Troubleshooting

### Email not sending?

Check environment variable:

```bash
echo $ALERT_EMAIL_PASS
```

If empty:
```bash
source /etc/profile
```

Make sure:
- Gmail App Password is used
- SMTP access is allowed

---

## 🔐 Security Recommendations

- Do NOT hardcode passwords in the script.
- Use App Password instead of main Gmail password.
- Restrict access to the script file.
- Monitor logs regularly.

---

## ✅ Result

After setup:
- Every SSH login triggers the script
- Email alert is sent
- Activity is logged
- Whitelisted IPs are ignored

Your SSH login monitoring system is now fully automated.
