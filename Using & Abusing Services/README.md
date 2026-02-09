# Using & Abusing Services

As I work more with Linux systems, I start to see that services are constantly running in the background. These services handle things like networking, logging, printing, and remote access. Understanding how to manage services is important — but understanding how they can be abused is critical from a security perspective. In this section, I break down how services work, how to control them, and why misconfigured services are common attack targets.

---

## What Are Services?

Services (also called daemons) are programs that run in the background.

Examples:
- ssh → remote access
- apache → web server
- cron → scheduled tasks
- mysql → database service

They usually start at boot and keep running until stopped.

---

## Listing Running Services

On systemd-based systems:
systemctl list-units --type=service

To see all services (including inactive):
systemctl list-unit-files --type=service

This helps me understand what’s running and what’s enabled.

---

## Service States

Common service states:
- active (running)
- inactive (stopped)
- failed
- enabled (starts at boot)
- disabled (does not start at boot)

Knowing the difference between active and enabled is important.

---

## Managing Services with systemctl

Start a service:
sudo systemctl start service_name

Stop a service:
sudo systemctl stop service_name

Restart a service:
sudo systemctl restart service_name

Check status:
systemctl status service_name

---

## Enabling and Disabling Services

Enable a service at boot:
sudo systemctl enable service_name

Disable a service at boot:
sudo systemctl disable service_name

Disable and stop immediately:
sudo systemctl disable --now service_name

This reduces unnecessary attack surface.

---

## Legacy Service Management

Older systems may use:
service service_name start
service service_name stop

Or:
chkconfig

These still exist on some systems.

---

## Why Services Are Attack Targets

Services often:
- Run as root or privileged users
- Listen on network ports
- Use configuration files with weak permissions
- Are forgotten after installation

Attackers look for:
- Outdated services
- Default credentials
- Misconfigurations
- Overly permissive file access

---

## Discovering Exposed Services

To see listening services:
ss -tulpn

Or:
netstat -tulpn

This shows which services are exposed to the network.

---

## Abusing Misconfigured Services

Common abuse scenarios:
- SSH running with weak authentication
- Web servers running as root
- Services with writable config files
- Cron jobs running scripts from writable directories

These misconfigurations can lead to privilege escalation.

---

## Service Configuration Files

Service configs are usually stored in:
- /etc/
- /etc/service_name/

Example:
- /etc/ssh/sshd_config
- /etc/apache2/apache2.conf

Permissions on these files matter.

---

## Services and Privilege Escalation

If a service:
- Runs as root
- Executes writable scripts
- Uses insecure paths

It can be abused to gain higher privileges.

This is a common post-exploitation technique.

---

## Stopping Unnecessary Services

To reduce risk:
- Disable services you don’t need
- Remove unused packages
- Monitor service changes

Fewer services = smaller attack surface.

---

## Practical Service Troubleshooting

Here’s how I approach service issues:

If a service won’t start:
systemctl status service_name
journalctl -u service_name

If a service shouldn’t be running:
systemctl disable --now service_name

If I suspect exposure:
ss -tulpn

---

## Security Mindset with Services

Always ask:
- Does this service need to run?
- Does it need network access?
- Does it run with least privilege?
- Are its config files protected?

These questions prevent most service-related issues.

---

## Good Service Management Habits

- Regularly review enabled services
- Keep services updated
- Restrict permissions on config files
- Monitor logs for unusual activity
- Disable what you don’t use

---

## Key Takeaway

Services are powerful and necessary, but they are also a common attack vector when mismanaged. Understanding how to control, inspect, and secure services gives me both system stability and a strong security advantage.
