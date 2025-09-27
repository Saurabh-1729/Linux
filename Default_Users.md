
---

## 👤 **Default Users in Linux**

1. **root**

   * The superuser (full control over the system).
   * UID (User ID) = `0`.
   * Can read/write everything, manage users, install software, etc.

2. **daemon / system users**

   * Special accounts created for running background services (daemons).
   * Example:

     * `daemon` → generic services
     * `bin` → for binaries
     * `sys` → for system files
     * `www-data` → for web servers like Apache/Nginx
     * `mysql` or `postgres` → for database servers
   * These users usually **cannot log in** (their shell is `/usr/sbin/nologin` or `/bin/false`).
   * Purpose: to isolate services for security.

3. **nobody**

   * A very restricted user with almost no privileges.
   * UID often = `65534`.
   * Used for running untrusted processes safely.

4. **first login user (you)**

   * During installation, Linux asks you to create your own user (e.g., `saurabh`).
   * This becomes your normal login account.
   * Usually added to `sudo` group so you can run admin commands with `sudo`.

---

## 🔎 How to see all users on your system

```bash
cat /etc/passwd
```

You’ll see entries like:

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
...
saurabh:x:1000:1000:Saurabh,,,:/home/saurabh:/bin/bash
```

Format:

```
username:password:UID:GID:info:home_directory:shell
```

---

✅ **In summary**:

* Always present → `root`, `nobody`, daemon/system users.
* You add your own user during installation.
* Many daemons (services) create their own users automatically.

---

Would you like me to also give you the list of **default groups** that come with Linux (like `root`, `sudo`, `adm`, `wheel`)?
