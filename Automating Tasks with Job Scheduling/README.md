# Automating Tasks with Job Scheduling

Linux provides multiple mechanisms for automating tasks, ranging from one-time delayed jobs to recurring system-wide schedules. Mastering job scheduling is essential for maintenance, backups, monitoring, and long-running system administration workflows. This section covers cron, at, systemd timers, and practical automation patterns.

## Cron: Recurring Task Scheduling
cron is the primary service for running commands on a repeating schedule.

### View Current User’s Cron Jobs
crontab -l

### Edit User’s Crontab
crontab -e

### Edit System-Wide Crontab
sudo nano /etc/crontab

### Cron Time Format
Each cron entry uses five time fields:
1. Minute (0–59)
2. Hour (0–23)
3. Day of Month (1–31)
4. Month (1–12)
5. Day of Week (0–7, where 0/7 = Sunday)

### Examples
Run a script every day at 2 AM:
0 2 * * * /usr/local/bin/backup.sh

Run every 5 minutes:
*/5 * * * * /usr/local/bin/check_status.sh

Run every Monday at 9 AM:
0 9 * * 1 /usr/local/bin/report.sh

Run at reboot:
@reboot /usr/local/bin/startup_tasks.sh

## Cron Logging & Troubleshooting
Cron logs are essential for verifying execution.

### View Cron Logs
journalctl -u cron  
or:  
grep CRON /var/log/syslog

### Common Issues
- Wrong PATH → specify full paths in scripts  
- Permissions → script must be executable  
- Environment differences → cron runs with a minimal environment  

To debug, capture output:
*/10 * * * * /usr/local/bin/task.sh >> /var/log/task.log 2>&1

## at: One-Time Scheduled Jobs
at schedules a command to run once at a specific time.

### Check if atd Service Is Running
sudo systemctl status atd

### Schedule a Job
echo "shutdown -r now" | at 3am

### List Pending Jobs
atq

### Remove a Job
atrm <job_id>

### Examples
Run a script in 30 minutes:
echo "/usr/local/bin/scan.sh" | at now + 30 minutes

Run at a specific date:
echo "tar -czf /backup/home.tar.gz /home" | at 23:00 02/10/2026

## systemd Timers: Modern Scheduling
systemd timers are more powerful and flexible than cron. They support calendar expressions, accuracy guarantees, and integration with system services.

### Timer Components
A timer consists of:
- A service unit (what to run)
- A timer unit (when to run)

### Example: Create a Service
/etc/systemd/system/cleanup.service
[Unit]
Description=Cleanup Temporary Files
[Service]
Type=oneshot
ExecStart=/usr/local/bin/cleanup.sh

### Create the Timer
/etc/systemd/system/cleanup.timer
[Unit]
Description=Run cleanup daily
[Timer]
OnCalendar=daily
Persistent=true
[Install]
WantedBy=timers.target

### Enable & Start the Timer
sudo systemctl enable --now cleanup.timer

### Check Timer Status
systemctl list-timers

### Common OnCalendar Expressions
- daily → once per day  
- hourly → once per hour  
- weekly → once per week  
- Mon *-*-* 09:00:00 → every Monday at 9 AM  
- *-*-01 00:00:00 → first day of every month  

## Environment Considerations
Cron and systemd run with limited environments.

### Best Practices
- Use absolute paths (/usr/bin/python3, not python3)
- Set PATH manually if needed:
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
- Ensure scripts are executable:
chmod +x script.sh
- Log output for debugging

## Automating Common Tasks

### Backups
0 1 * * * rsync -a /home /backup

### Log Rotation
sudo logrotate -f /etc/logrotate.conf

### System Updates
0 3 * * 0 apt update && apt upgrade -y

### Monitoring & Alerts
*/10 * * * * /usr/local/bin/check_disk.sh | mail -s "Disk Alert" admin@example.com

## Security Considerations

### Restrict Who Can Use Cron
/etc/cron.allow  
/etc/cron.deny

### Restrict at Usage
/etc/at.allow  
/etc/at.deny

### Protect Scripts
- Store automation scripts in root-owned directories  
- Use least privilege  
- Avoid embedding credentials  

### Monitor Timer & Cron Activity
journalctl -u cron  
journalctl -u <timer>.service

## Practical Troubleshooting Workflow
If a scheduled job doesn’t run:

1. Check logs  
journalctl -u cron  
journalctl -u <timer>.service  

2. Verify permissions  
Script executable, correct ownership  

3. Check paths  
Use absolute paths, confirm environment variables  

4. Run manually  
/usr/local/bin/script.sh  

5. Capture output  
/var/log/script.log 2>&1

## Key Takeaway
Linux provides multiple automation tools — cron for recurring tasks, at for one-time jobs, and systemd timers for modern, reliable scheduling. Understanding these mechanisms allows you to automate maintenance, monitoring, backups, and system workflows with precision and control.
