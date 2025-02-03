# Carbonio Files Watches Service Configuration Issue

## Description

This template addresses issues with the `carbonio-files-watches.service`, focusing on common problems with file system monitoring, permissions, and service configuration.

## Issue Symptoms

- [ ] Service fails to start
- [ ] Permission denied errors in logs
- [ ] Inotify watch limit errors
- [ ] PID file creation failures
- [ ] Service repeatedly restarting

## Quick Solution

```bash
# 1. Set up directories and permissions
sudo mkdir -p /opt/carbonio/files /var/log/carbonio /var/run/carbonio-files
sudo groupadd -f carbonio-files
sudo useradd -r -g carbonio-files carbonio-files 2>/dev/null || true
sudo chown -R carbonio-files:carbonio-files /opt/carbonio/files /var/log/carbonio /var/run/carbonio-files
sudo chmod 755 /opt/carbonio/files /var/log/carbonio /var/run/carbonio-files

# 2. Configure inotify limits
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

# 3. Install dependencies
sudo apt-get install inotify-tools netcat

# 4. Restart service
sudo systemctl daemon-reload
sudo systemctl restart carbonio-files-watches.service
```

## Verification Steps

```bash
# Check service status
sudo systemctl status carbonio-files-watches.service

# Verify log file creation
ls -la /var/log/carbonio/files-watches.log

# Check inotify watches
cat /proc/sys/fs/inotify/max_user_watches
```

## Debugging Information

Please provide:

1. Service Status:
```bash
sudo systemctl status carbonio-files-watches.service
```

2. Journal Logs:
```bash
sudo journalctl -u carbonio-files-watches.service -n 50
```

3. File Permissions:
```bash
ls -la /opt/carbonio/files
ls -la /var/log/carbonio
ls -la /var/run/carbonio-files
```

4. System Information:
- OS Version:
- Carbonio Version:
- Available System Resources:
- Current inotify Watch Limit:

## Common Error Messages

1. Permission Denied:
```
tee: /var/log/carbonio/files-watches.log: Permission denied
```

2. PID File Error:
```
cannot create /var/run/carbonio-files-watches.pid
```

3. Inotify Limit Error:
```
Failed to increase max_user_watches
```

## Additional Checks

- [ ] Verify carbonio-files user exists
- [ ] Check directory permissions
- [ ] Confirm inotify-tools installation
- [ ] Verify system resources
- [ ] Check for conflicting services

## Labels

- configuration
- file-system
- monitoring
- service
- systemd

## Related Documentation

- Link to main Carbonio documentation
- Link to systemd service configuration
- Link to inotify documentation

---

Remember to replace placeholders with actual values and add any specific details relevant to your environment.
