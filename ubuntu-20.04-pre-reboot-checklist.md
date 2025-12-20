# Pre-Reboot Checklist for Ubuntu 20.04 LTS

This document provides a step-by-step checklist to safely restart an Ubuntu 20.04 server and identify running and critical services before rebooting.

---

## 1. Check Running Services

List all currently running systemd services:

`systemctl list-units --type=service --state=running`

List all enabled services (these will start automatically after reboot):

`systemctl list-unit-files --type=service --state=enabled`

---

## 2. Verify Critical Services

Check the status of commonly critical services:

`systemctl status ssh`  
`systemctl status nginx`  
`systemctl status apache2`  
`systemctl status mysql`  
`systemctl status docker`  
`systemctl status kubelet`  
`systemctl status cron`

Ensure SSH is enabled to start on boot:

`systemctl is-enabled ssh`

Expected output:

`enabled`

---

## 3. Check Listening Ports

Identify services listening on network ports:

`ss -tulpen`

If net-tools is installed:

`netstat -tulpen`

Common ports to verify:

- `22`   : SSH  
- `80`   : HTTP  
- `443`  : HTTPS  
- `3306` : MySQL  
- `5432` : PostgreSQL  

---

## 4. Check Logged-in Users and Uptime

Check system uptime:

`uptime`

Check logged-in users:

`who`

Check active SSH sessions:

`ss -tnp | grep ssh`

---

## 5. Verify Disk Space

Check disk usage:

`df -h`

Ensure `/` and `/boot` are not full.

Check inode usage:

`df -i`

---

## 6. Check Reboot Requirement and Pending Updates

Check if a reboot is required:

`ls -l /var/run/reboot-required`

Simulate pending upgrades:

`apt -s upgrade`

---

## 7. Check for Failed Services

Identify failed services:

`systemctl --failed`

Investigate and fix any failed units before rebooting.

---

## 8. Review Previous Boot Errors

Check high-priority errors from the last boot:

`journalctl -p 3 -xb`

---

## 9. Confirm Auto-Start for Important Services

Verify if a service is enabled at boot:

`systemctl is-enabled <service-name>`

Enable it if required:

`systemctl enable <service-name>`

---

## 10. SSH Safety Check (Critical)

Before rebooting a remote server, always verify:

`systemctl status ssh`  
`ss -tulpen | grep :22`

---

## 11. Reboot the Server

Once all checks are completed:

`reboot`

Or:

`shutdown -r now`
