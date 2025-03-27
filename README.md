![GitHub last commit](https://img.shields.io/github/last-commit/Eli-Zac/Cloudflare-Dynamic-IP-Updater?style=for-the-badge&color=orange)
![GitHub watchers](https://img.shields.io/github/watchers/Eli-Zac/Cloudflare-Dynamic-IP-Updater?style=for-the-badge&color=orange)
![GitHub Repo stars](https://img.shields.io/github/stars/Eli-Zac/Cloudflare-Dynamic-IP-Updater?style=for-the-badge&color=orange)

# Cloudflare Dynamic IP Updater
A Python script to automatically update Cloudflare DNS records with your dynamic IP, supporting proxy toggling and multiple records.

## Features
- Automatically fetches the current public IP using the `ipify` service.
- Dynamically retrieves DNS record IDs from Cloudflare, removing manual setup requirements.
- Updates multiple DNS records based on configuration.
- Supports enabling or disabling Cloudflare Proxy (`proxied` option) for each record.
- Runs continuously, checking for IP changes at regular intervals.

## Requirements
- Python 3.7 or higher
- `requests` library

## Setup Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/Eli-Zac/cloudflare-dynamic-ip-updater.git
cd cloudflare-dynamic-ip-updater
```

### 2. Install Dependencies
Install the required Python library:
```bash
pip install reqirements.txt
```

### 3. Create a Configuration File
Use the provided `config.json.example` as a template:
```bash
cp config.json.example config.json
```
Edit config.json with your details:
```json
{
    "records": [
        {
            "api_token": "your_api_token",
            "zone_id": "your_zone_id",
            "record_name": "example.com",
            "proxied": true
        }
    ]
}
```
**api_token:** Your Cloudflare API Token with permissions for DNS Zone and DNS Records.<br>
**zone_id:** The Zone ID of your domain in Cloudflare.<br>
**record_name:** The DNS record (e.g., example.com or sub.example.com).<br>
**proxied:** true to enable Cloudflare Proxy, false to disable it.<br>

### 4. Run the Script
Run the script to start updating your DNS records:
```
python main.py
```
The script will fetch your current public IP and update the specified DNS records if the IP changes.

### 5. Run Continuously
To keep the script running 24/7, use a process manager like screen, tmux, or a system service.
#### Using `screen`:
```
screen -S ip-updater python cloudflare_ip_updater.py
```
#### Using a Systemd Service (Linux):
Create a service file (e.g., `/etc/systemd/system/cloudflare-ip-updater.service`):
```ini
[Unit]
Description=Cloudflare Dynamic IP Updater
After=network.target

[Service]
ExecStart=/usr/bin/python3 /path/to/cloudflare_ip_updater.py
Restart=always
User=your_username
WorkingDirectory=/path/to/repo

[Install]
WantedBy=multi-user.target
```
Enable and start the service:
```bash
sudo systemctl enable cloudflare-ip-updater
sudo systemctl start cloudflare-ip-updater
```

---
## Configuration Example
`config.json`:
```json
{
    "records": [
        {
            "api_token": "your_api_token",
            "zone_id": "your_zone_id",
            "record_name": "example.com",
            "proxied": true
        },
        {
            "api_token": "another_api_token",
            "zone_id": "another_zone_id",
            "record_name": "sub.example.com",
            "proxied": false
        }
    ]
}
```
