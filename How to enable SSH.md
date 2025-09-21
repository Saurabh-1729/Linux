Great — here’s a practical, no-fluff guide so other users can log in to *your Linux machine*, what must be present, and how to do it securely. I’ll give you (A) the prerequisites checklist, (B) step-by-step commands (both password & key based), (C) common server config checks, and (D) security/extra tips.

# ✅ Prerequisites (what must exist)

* The machine is **reachable** over the network (IP or DNS) or users have local/console access.
* **SSH server** installed & running if you want remote SSH logins (OpenSSH is standard).
* A **user account** for each person (an entry in `/etc/passwd`).
* A **valid shell** (e.g. `/bin/bash`) and a **home directory** (usually `/home/username`).
* Proper **file permissions** on user home and `~/.ssh/authorized_keys` for key auth.
* Firewall allows the SSH port (default 22) or appropriate port.
* If using centralized auth: LDAP/Active Directory/PAM must be configured.

---

# 🛠️ Quick steps — create a user (Debian/Ubuntu / RHEL/CentOS)

## Debian/Ubuntu (easy)

Create user, set password, create home automatically:

```bash
sudo adduser alice
# follow prompts to set password and full name
```

Or noninteractive:

```bash
sudo useradd -m -s /bin/bash alice
sudo passwd alice
```

## RHEL/CentOS

```bash
sudo adduser alice
sudo passwd alice
# or
sudo useradd -m -s /bin/bash alice
sudo passwd alice
```

`-m` creates the home directory, `-s` sets the shell.

---

# 🔑 Enable SSH login (if not already)

Install & start OpenSSH server:

### Debian/Ubuntu

```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl enable --now ssh
```

### RHEL/CentOS

```bash
sudo yum install -y openssh-server
sudo systemctl enable --now sshd
```

Check status:

```bash
sudo systemctl status sshd   # or `ssh` on Ubuntu
```

Open firewall port (example `ufw` and `firewalld`):

```bash
# ufw (Ubuntu)
sudo ufw allow 22/tcp
# firewalld (RHEL)
sudo firewall-cmd --add-service=ssh --permanent
sudo firewall-cmd --reload
```

---

# 🔐 Recommended: use SSH key authentication (safer than passwords)

## On the client (user machine)

Generate key (if they don’t have):

```bash
ssh-keygen -t ed25519 -C "alice@myhost"      # or rsa: -t rsa -b 4096
# Accept defaults; private key ~/.ssh/id_ed25519, public key ~/.ssh/id_ed25519.pub
```

## Copy public key to server (easy)

```bash
ssh-copy-id alice@server-ip
# or manually:
# create ~/.ssh and authorized_keys, set permissions:
ssh alice@server-ip "mkdir -p ~/.ssh && chmod 700 ~/.ssh"
# on client
cat ~/.ssh/id_ed25519.pub | ssh alice@server-ip 'cat >> ~/.ssh/authorized_keys'
ssh alice@server-ip "chmod 600 ~/.ssh/authorized_keys && chown -R alice:alice ~/.ssh"
```

Now login:

```bash
ssh alice@server-ip
```

---

# 🧰 Important config checks on the server (`/etc/ssh/sshd_config`)

Open `/etc/ssh/sshd_config` and ensure these (restart sshd after changes):

* Allow key auth:

```text
PubkeyAuthentication yes
```

* Password auth (if you want to allow passwords):

```text
PasswordAuthentication yes     # or no to disable password login
```

* Limit users (optional):

```text
AllowUsers alice bob            # only these users can SSH
# or
DenyUsers root
```

* Disable root login:

```text
PermitRootLogin no
```

Restart:

```bash
sudo systemctl restart sshd
```

---

# 🧾 Give sudo access (if required)

To allow user `alice` to run sudo:

```bash
sudo usermod -aG sudo alice      # Debian/Ubuntu (sudo group)
# or for RHEL:
sudo usermod -aG wheel alice     # wheel group is used for sudoers
```

Edit sudoers safely:

```bash
sudo visudo
# add: alice ALL=(ALL) NOPASSWD:ALL    # or without NOPASSWD for password prompting
```

---

# 🪪 Extras & administration controls

* **Temporary accounts / expiry**:

```bash
sudo chage -E 2025-10-01 alice   # expire account on that date
```

* **Lock an account**:

```bash
sudo usermod -L alice             # lock
sudo usermod -U alice             # unlock
```

* **Set a password policy**: install `libpam-pwquality` / configure `/etc/pam.d/common-password`.
* **Central auth**: If many users, consider LDAP / FreeIPA / AD for centralized login.
* **Home directory disk quota**: use `quota` tools to limit usage.

---

# 🔒 Security best practices

* Prefer **key-based auth** and disable `PasswordAuthentication` in `sshd_config`.
* **Disable root SSH login** (`PermitRootLogin no`).
* Use **fail2ban** to block repeated brute-force attempts.
* Keep SSH on a nonstandard port only as obscurity (not a substitute for security).
* Use `AllowUsers` or `AllowGroups` to restrict who can SSH.
* Regularly update the system and OpenSSH package.

---

# 👥 Local vs Remote GUI logins

* SSH gives a **shell** (terminal). For GUI remote desktop use **RDP**, VNC, or Chrome Remote Desktop.
* You can forward GUI apps over SSH (`ssh -X`) but that’s different and performance varies.

---

# ✅ Quick checklist before telling others to log in

* [ ] `adduser username` done and password set (or keys installed).
* [ ] SSH server installed & running.
* [ ] Firewall allows the SSH port.
* [ ] `~/.ssh/authorized_keys` exists with correct permissions (600) and `~/.ssh` (700).
* [ ] `sshd_config` allows desired auth methods and root login disabled.
* [ ] (Optional) User added to `sudo` or required groups.

---

If you tell me:

* which Linux distro you’re running (Ubuntu/CentOS/etc.), and
* whether you want to allow **passwords** or **only keys**,

I’ll give you the exact commands tailored to your machine and a short checklist you can copy & paste for teammates.
