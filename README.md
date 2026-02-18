# 🚀 Teleport Login Journal Watcher

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Systemd](https://img.shields.io/badge/Systemd-Service-green)
![Teleport](https://img.shields.io/badge/Teleport-Session_Monitor-orange)
![Status](https://img.shields.io/badge/Status-Production_Ready-brightgreen)

A real-time monitoring service that watches Teleport logs using `journalctl` and sends email alerts whenever a new Teleport session starts.

---

## 📌 Features

- ✅ Real-time Teleport session monitoring
- 📧 Email alert on every session start
- 📝 Logs login details to file
- 🔁 Runs as a systemd service
- 🔒 Uses secure environment variable for email password
- 🖥 Captures hostname & private IP automatically

---

# 📂 File Locations

| File | Path |
|------|------|
| Python Script | `/usr/local/bin/teleport_login_journal_watcher.py` |
| Log File | `/var/log/teleport_login.log` |
| Systemd Service | `/etc/systemd/system/teleport-login-watcher.service` |

---

# ⚙️ Setup Instructions

---

## 1️⃣ Create / Update the Python Script

```bash
sudo vim /usr/local/bin/teleport_login_journal_watcher.py
```

Paste the script content.

---

## 2️⃣ Update Script Configuration (IMPORTANT)

Inside the script, update:

### 🔹 Sender Email
```python
EMAIL_FROM = "your_email@gmail.com"
```

### 🔹 Receiver Emails
```python
EMAIL_TO = [
    "admin@example.com",
    "security@example.com"
]
```

⚠️ Replace with your actual email IDs.

---

## 3️⃣ Set Proper Permissions

Based on your environment:

### Script Permissions
```bash
sudo chmod 755 /usr/local/bin/teleport_login_journal_watcher.py
sudo chown root:root /usr/local/bin/teleport_login_journal_watcher.py
```

Expected:
```
-rwxr-xr-x 1 root root teleport_login_journal_watcher.py
```

---

### Log File Permissions
```bash
sudo touch /var/log/teleport_login.log
sudo chmod 644 /var/log/teleport_login.log
sudo chown root:root /var/log/teleport_login.log
```

Expected:
```
-rw-r--r-- 1 root root teleport_login.log
```

---

## 4️⃣ Configure Email Password (Environment Variable)

⚠️ Use **Gmail App Password** (NOT your main Gmail password)

Edit:

```bash
sudo vim /etc/environment
```

Add:

```
ALERT_EMAIL_PASS=your_gmail_app_password
```

Save and exit.

---

## 5️⃣ Create Systemd Service

Create the service file:

```bash
sudo vim /etc/systemd/system/teleport-login-watcher.service
```

Paste:

```
[Unit]
Description=Teleport Login Journal Watcher
After=teleport.service

[Service]
EnvironmentFile=/etc/environment
ExecStart=/usr/bin/python3 /usr/local/bin/teleport_login_journal_watcher.py
Restart=always
RestartSec=2

[Install]
WantedBy=multi-user.target
```

Save and exit.

---

## 6️⃣ Reload Systemd

```bash
sudo systemctl daemon-reload
```

---

## 7️⃣ Start the Service

```bash
sudo systemctl start teleport-login-watcher
```

---

## 8️⃣ Check Service Status

```bash
sudo systemctl status teleport-login-watcher
```

You should see:
```
active (running)
```

---

## 9️⃣ Enable Service at Boot

```bash
sudo systemctl enable teleport-login-watcher
```

Now it will automatically start after every reboot.

---

# 🧪 Testing

1. Login using Teleport UI or CLI.
2. Check log file:

```bash
cat /var/log/teleport_login.log
```

Example output:

```
====================================
TELEPORT LOGIN DETECTED
Teleport User : rajat
OS Login      : ubuntu
Client IP     : 14.xx.xx.xx
Server        : nxsam-test
Private IP    : 10.xx.xx.xx
Cluster       : prod-cluster
Session ID    : abcdef-12345
Time (UTC)    : 2026-02-18T10:30:00
====================================
EMAIL STATUS : Sent successfully
```

---

# 🛠 Troubleshooting

### Service not starting?

Check logs:

```bash
journalctl -u teleport-login-watcher -f
```

---

### Email not sending?

Verify:

```bash
echo $ALERT_EMAIL_PASS
```

If empty:
- Ensure `/etc/environment` contains the variable
- Restart the service:

```bash
sudo systemctl restart teleport-login-watcher
```

---

# 🔐 Security Best Practices

- ❌ Never hardcode passwords in script
- ✅ Use Gmail App Password
- ✅ Keep script owned by root
- ✅ Restrict log file permissions
- ✅ Monitor logs regularly

---

# 📊 Architecture Flow

```
Teleport Login
        ↓
journalctl -u teleport -f
        ↓
Python Watcher Script
        ↓
Log Written → /var/log/teleport_login.log
        ↓
Email Alert Sent
```

---

# 🎯 Final Result

After setup:

- Every Teleport session start is detected
- Login details are logged
- Email alerts are sent
- Service auto-starts at boot
- Fully production-ready monitoring

---

## 👨‍💻 Author

Rajat Kumar  
DevOps Engineer  

---

⭐ If this project helped you, consider starring the repository.
