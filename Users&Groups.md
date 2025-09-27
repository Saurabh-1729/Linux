Great question 👍 Let’s break this down step by step.

---

## 👥 Creating and Using Your Own Group

### 1. **Create a new group**

Only the **system administrator (root)** can create new groups.

```bash
sudo groupadd mygroup
```

This adds a new group called `mygroup`.

---

### 2. **Add yourself (or others) to the group**

```bash
sudo usermod -aG mygroup saurabh
```

* `-aG` means “append to supplementary groups.”
* After running this, log out and back in (or run `newgrp mygroup`) for it to take effect.

---

### 3. **Create files with group ownership**

```bash
touch report.txt
ls -l report.txt
```

By default, the file’s group will be your **primary group** (often the same as your username).

To set a specific group:

```bash
chown :mygroup report.txt
```

Now `report.txt` belongs to group `mygroup`.

---

### 4. **Set permissions for group**

Example: give group members read/write access:

```bash
chmod 660 report.txt
```

This makes the file accessible to:

* Owner → read/write
* Group → read/write
* Others → none

---

### 5. **Share files in a directory with your group**

If you want all new files in a folder to inherit the group:

```bash
mkdir shared_dir
chgrp mygroup shared_dir
chmod 2770 shared_dir
```

* `2` → setgid bit: new files inside `shared_dir` will automatically get group `mygroup`.
* `770` → owner + group full access, others none.

---

## 🛠️ Is this the job of a **system administrator**?

* **Yes**: Normally, only admins (root/sudo) can create groups and manage user memberships.
* **But**: If it’s *your own system* (personal Linux), you can do it yourself with `sudo`.
* In a **company or server environment**, you usually request the sysadmin to create groups and add you.

---

✅ In summary:

* You can create groups (with root/sudo).
* Add yourself/others.
* Control access to files and folders via group ownership and permissions.

---

Do you want me to show you a **practical mini-project** where you and another “user” share files only within your group (but others can’t see them)?
