A **symlink** (symbolic link) is like a *shortcut* or *pointer* to another file or folder.  
It lets you access something located far away in the filesystem using a short, convenient path.

***

# ✅ **What is a Symlink? (In Simple Words)**

A **symbolic link** is:

*   Like a **Windows shortcut**
*   A small file that *points* to another file/folder
*   Lets you run programs without typing the full path
*   Safe to delete (it won’t delete the original file)

Example:  
Instead of typing:

    /opt/idea-IU-253.29346.240/bin/idea.sh

You can create a symlink called `idea`:

    /usr/local/bin/idea

Now you just type:

    idea

…from anywhere in the system.

***

# ✅ **How to Create a Symlink (Linux)**

General syntax:

```bash
ln -s <target_file_or_folder> <shortcut_link>
```

Where:

*   `<target_file_or_folder>` = original file
*   `<shortcut_link>` = where you want your symlink

***

# 🎯 **Example: Create symlink for IntelliJ IDEA**

To create a command `idea` you can run anywhere:

```bash
sudo ln -s /opt/idea-IU-253.29346.240/bin/idea.sh /usr/local/bin/idea
```

Now run:

```bash
idea &
```

✔ No need to `cd`  
✔ Runs in background  
✔ Works from any folder

***

# 🧐 **Check Symlink Exists**

```bash
ls -l /usr/local/bin/idea
```

You should see something like:

    idea -> /opt/idea-IU-253.29346.240/bin/idea.sh

***

# ❌ **How to Remove a Symlink**

If you want to remove it:

```bash
sudo rm /usr/local/bin/idea
```

This **does NOT delete** the original IntelliJ installation.

***

# 🔍 Types of Links (for clarity)

| Type          | Name          | Behavior                                                            |
| ------------- | ------------- | ------------------------------------------------------------------- |
| **Soft link** | Symbolic link | Points to original. Breaks if original is deleted.                  |
| **Hard link** | Hard link     | Another reference to same file data. Safer but not for directories. |

You mainly use **soft links** for applications.

***

# If you want, I can create symlinks for other tools too

(Java, Tomcat, GlassFish, etc.)—just tell me!
