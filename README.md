# FSA Auditor (Alpha)

FSA Auditor - because nobody remembers to check if their server is actually secure.
<img width="544" height="135" alt="{9E45B988-3CDE-4DA2-A3FD-7F27A195FEE0}" src="https://github.com/user-attachments/assets/fc203c71-e5a8-47e3-90da-155cea996eff" />

## What's This?

Got a VPS? Forgot to secure it properly? Yeah, we've all been there. This script checks if you've left the digital equivalent of your front door wide open.

FSA Auditor runs a comprehensive security check on your Linux server and tells you what's wrong before the hackers do. It's like a health checkup for your VPS, except it actually gives you straight answers.

## What It Checks

**SSH Configuration**
- Is root login enabled? (spoiler: it probably is)
- Still using password authentication? (yikes)
- Running on port 22? (welcome to bot city)
- All the stuff you were supposed to fix after setup but forgot

**Firewall Status**
- Do you even have a firewall running?
- UFW and iptables support
- Shows you what ports you've opened

**Open Ports**
- Scans what's actually listening
- Flags weird stuff that probably shouldn't be public
- Checks if your database is exposed to the entire internet (it happens more than you'd think)

**User Accounts**
- Who has sudo access
- Empty passwords (please no)
- Multiple root users (why though)

**Docker Security**
- Which containers are running as root (probably all of them)
- What ports you're exposing
- Daemon configuration

**Database Checks**
- PostgreSQL configuration
- MySQL/MariaDB setup
- Redis security (or lack thereof)
- Whether they're bound to localhost or screaming into the void

**System Health**
- Pending updates
- Unattended-upgrades status
- Security tools (Fail2ban, AppArmor, SELinux)

**File Permissions**
- Critical system files
- World-writable stuff in /etc

## Usage

```bash
# Clone it
git clone https://github.com/fromgodd/FSA-Auditor.git
cd FSA-Auditor

# Make it executable
chmod +x fsaauditor.sh

# Run it (needs root)
sudo ./fsaauditor.sh
```

Want to save the output?
```bash
sudo ./fsaauditor.sh | tee audit-$(date +%Y%m%d).log
```

## What You'll See

The script gives you a color-coded report:
- 🟢 Green = you did something right
- 🔴 Red = fix this now
- 🟡 Yellow = should probably look at this

At the end, you get a security score. If it's below 60%, you've got work to do.

## Requirements

- Linux (tested on Ubuntu 22.04, should work on most distros)
- Root access
- Basic understanding that security matters

Optional but recommended:
- Docker (if you want container checks)
- PostgreSQL/MySQL/Redis (if you want database checks)
- Fail2ban (you should have this anyway)

## Example Output

```
Security Score: 44% - NEEDS IMPROVEMENT

✗ Root login is ENABLED
✗ Password authentication is ENABLED
⚠ PostgreSQL listening on public interface
⚠ 5 containers running as root
⚠ Fail2ban installed but not running
```

Yeah, that's a real audit result. Don't be that person.

## Common Issues You'll Find

1. **SSH still allows passwords** - Set up SSH keys, seriously
2. **Root login enabled** - This is hacker Christmas
3. **Databases exposed to internet** - Why would you do this
4. **No firewall** - At least pretend to try
5. **Containers running as root** - Docker isn't magic security
6. **System not updated** - Those updates exist for a reason

## Contributing

Found a bug? Got ideas? PRs welcome. This is alpha, so there's definitely room for improvement.

## Disclaimer

This tool tells you what's wrong. It doesn't fix it for you. That's on purpose - you should understand what you're changing on your server.

Also, this isn't a replacement for actual security practices. It's a starting point. Don't run a production server based solely on what a bash script tells you.

## License

MIT - do whatever you want with it

---

Run this regularly. Your future self will thank you when you're not dealing with a compromised server at 3 AM.
