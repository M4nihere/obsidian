The `apt-daily-upgrade.timer` is a **systemd timer** that schedules the `apt-daily-upgrade.service`, which handles **automatic daily upgrades** of packages on a system running Ubuntu (or other Debian-based distributions). It is part of the `unattended-upgrades` mechanism.

### Key Details:

- **Service Triggered**:  
    The timer triggers `apt-daily-upgrade.service`. This service installs updates for installed packages, including security updates and other package upgrades.
    
- **Default Configuration**: The configuration of the timer can be found in the system-provided unit file at `/lib/systemd/system/apt-daily-upgrade.timer`. It typically runs daily with a randomized delay to avoid simultaneous updates across many systems.
    
    Example:
    
    ```ini
    [Unit]
    Description=Daily apt upgrade and clean activities
    After=apt-daily.timer
    
    [Timer]
    OnCalendar=*-*-* 6:00         # Scheduled for 6:00 AM
    RandomizedDelaySec=60m        # Adds a randomized delay up to 60 minutes
    Persistent=true               # Ensures the timer runs even if the system was off at the scheduled time
    ```
    
- **Purpose**:
    
    - Installs security updates and package upgrades automatically.
    - Cleans up old versions of packages (e.g., removing no-longer-needed packages).

---

### Timer Status Commands

- **Check Timer Status**:
    
    ```bash
    systemctl status apt-daily-upgrade.timer
    ```
    
- **List All Timers**:
    
    ```bash
    systemctl list-timers
    ```
    

---

### Disable or Modify the Timer

- **Disable** the timer if you don't want automatic upgrades:
    
    ```bash
    sudo systemctl disable apt-daily-upgrade.timer
    sudo systemctl stop apt-daily-upgrade.timer
    ```
    
- **Customize** the schedule: Create an override configuration:
    
    ```bash
    sudo systemctl edit apt-daily-upgrade.timer
    ```
    
    Add a custom schedule, e.g., run at 11 PM instead:
    
    ```ini
    [Timer]
    OnCalendar=*-*-* 23:00
    RandomizedDelaySec=0
    ```
    


Bonus:

```bash
 sudo journalctl -u mysql.service --since "2025-02-01 06:30:00" --until "2025-02-01 06:35:00"
 sudo journalctl --since "2025-02-01 06:30:00" --until "2025-02-01 06:35:00"
```