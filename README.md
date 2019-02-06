### LXC config for hosting Docker
Commented config files for LXC allowing to host Docker inside an LXC container with minimum allowed capabilities.

Uses lxc.cap.keep (whitelist) instead of lxc.cap.drop (blacklist) so that any new capability added to Linux kernel won't be automatically granted to LXC containers.

The list of capabilities is minimal. You may need to add more if you have special needs but this configuration should be enough for most use-cases. 

The configuration is split into several files:
 * **common.conf**: Sources default LXC config and makes a "base" for other files. Containers using this config file can run Systemd but the allowed capabilities are limited and some software may not start.
 * **docker.conf**: Basic configuration file to allow running default Docker and containers inside it (as of Aug 2018). Userns remap won't work with this config.
 * **docker.userns.conf**: Like docker.conf, but adds support for --userns-remap (UID/GID remaping of root user inside Docker containers).

#### Problems
Please note that I am unable to debug your EPERM "Permission denied" problems. It is your responsibility to understand what capabilities are required and identify them. You can use `strace` for that. 

Also, this configuration does not limit any additional security mechanisms (seccomp, apparmor). Normally they should be enabled as an additional security layer and eventually their config modified to suit your needs.

For AppArmor, don't forget to check out audit logs (auditd - /var/log/audit/audit.log) as denials are logged into the Kernel audit log. 

#### Contributing

Feel free to make a pull request or discuss improvements. 
My target is to support the latest official Docker with userns remap.
