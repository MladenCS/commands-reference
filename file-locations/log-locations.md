# Log File Locations

## Web Servers
* **Nginx**
  * Access Logs: `/var/log/nginx/access.log`
  * Error Logs: `/var/log/nginx/error.log`
* **Apache**
  * Access Logs: `/var/log/apache2/access.log`
  * Error Logs: `/var/log/apache2/error.log`
* **IIS (Windows)**
  * Access Logs: `C:\inetpub\logs\LogFiles\W3SVC1\`

## Databases
* **MySQL**
  * Error Logs: `/var/log/mysql/error.log`
  * General Query Logs: `/var/log/mysql/mysql.log`
  * Slow Query Logs: `/var/log/mysql/mysql-slow.log`
* **PostgreSQL**
  * Error and Activity Logs: `/var/log/postgresql/postgresql-{version}-main.log`
* **MongoDB**
  * Activity Logs: `/var/log/mongodb/mongod.log`
* **Redis**
  * Activity Logs: `/var/log/redis/redis-server.log`

## Web Applications
* **PHP**
  * Error Logs: `/var/log/php/error.log`
* **Node.js / PM2**
  * App Logs: `~/.pm2/logs/`

## Operating Systems
* **Linux**
  * General System Logs: `/var/log/syslog`
  * Authentication Logs: `/var/log/auth.log`
  * Kernel Logs: `/var/log/kern.log`
  * Boot Logs: `/var/log/boot.log`
  * Cron Logs: `/var/log/cron.log`
  * Package Manager Logs: `/var/log/dpkg.log`
  * journald (systemd): `journalctl`
* **Windows**
  * System Events: `Event Viewer → Windows Logs → System`
  * Security/Auth Events: `Event Viewer → Windows Logs → Security`
  * Application Events: `Event Viewer → Windows Logs → Application`
* **macOS**
  * System Logs: `/var/log/system.log`
  * Unified Log: `log show` / `Console.app`

## Firewalls & IDS/IPS
* **iptables**
  * Firewall Logs: `/var/log/iptables.log`
* **ufw**
  * Firewall Logs: `/var/log/ufw.log`
* **Snort**
  * Snort Logs: `/var/log/snort/`
* **Suricata**
  * Event Logs: `/var/log/suricata/eve.json`
  * Fast Logs: `/var/log/suricata/fast.log`

## Mail Servers
* **Postfix**
  * Mail Logs: `/var/log/mail.log`
  * Mail Errors: `/var/log/mail.err`
* **Dovecot**
  * Auth/Activity Logs: `/var/log/dovecot.log`

## SSH
* Auth/Login Logs (Debian/Ubuntu): `/var/log/auth.log`
* Auth/Login Logs (RHEL/CentOS): `/var/log/secure`
