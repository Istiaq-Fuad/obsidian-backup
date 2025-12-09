
The **Linux file system** is a hierarchical structure that organizes all files and directories under a single root directory `/`. It follows the **FHS (Filesystem Hierarchy Standard)**, which defines the purpose of each directory.

### **Key Characteristics**

- **Everything is a file** (including devices, directories, processes).
- Single-root structure: all partitions/devices are mounted under `/`.
- Case-sensitive: `File.txt` and `file.txt` are different.
- Uses permissions and ownership for security.

![[linux_file_system.png]]

# **Important Directories**

| Directory             | Purpose                                             |
| --------------------- | --------------------------------------------------- |
| **/**                 | Root directory; top of the file system tree         |
| **/bin**              | Essential user commands (e.g., `ls`, `cp`)          |
| **/sbin**             | System administration commands                      |
| **/etc**              | Configuration files for the system and services     |
| **/home**             | Home directories for users                          |
| **/root**             | Home directory of the root user                     |
| **/lib**, **/lib64**  | Shared libraries needed by system programs          |
| **/usr**              | User programs, libraries, documentation             |
| **/var**              | Variable data (logs, mail, spool files)             |
| **/tmp**              | Temporary files                                     |
| **/boot**             | Files required for booting (kernel, bootloader)     |
| **/dev**              | Device files (e.g., disks, USB, tty)                |
| **/proc**             | Virtual filesystem showing process and kernel info  |
| **/sys**              | Exposes kernel status and hardware details          |
| **/mnt** & **/media** | Mount points for removable or temporary filesystems |

# **Common File System Types in Linux**

- **ext4** – default in many Linux distros
- **xfs** – high-performance file system
- **btrfs** – advanced features (snapshots, compression)
- **tmpfs** – in-memory temporary file storage

# **Permissions and Ownership**

Each file has:

- **Owner**, **Group**
- **Permissions**: read (r), write (w), execute (x)

Set using commands like `chmod`, `chown`, `chgrp`.

# **Note on Linux File Types and Properties**

Linux treats **everything as a file**, and each file has specific _types_ and _properties_ that define how it behaves. These types and attributes are shown using commands like `ls -l`, `stat`, or file metadata.

## **1. Linux File Types**

In `ls -l`, the **first character** indicates the file type:

|Symbol|File Type|Description|
|---|---|---|
|**-**|Regular file|Text, binaries, documents, scripts|
|**d**|Directory|Contains files; similar to folders|
|**l**|Symbolic link|Points to another file (shortcut)|
|**c**|Character device|Hardware that transmits data char-by-char (e.g., keyboard, serial port)|
|**b**|Block device|Storage devices that read/write in blocks (e.g., hard disk, USB)|
|**p**|Named pipe (FIFO)|Enables process-to-process communication|
|**s**|Socket|Network/IPC communication endpoint|
|**D** (rare)|Door (Solaris)|Not common in Linux|

Example:

```
-rw-r--r--  file.txt      → regular file
drwxr-xr-x  folder/       → directory
lrwxrwxrwx  link → target → symbolic link
```

## **2. File Properties (Attributes)**

Every Linux file has the following properties:

### **a) Ownership**

- **User (owner)**
- **Group**
- Defines who controls the file.

View with:

```bash
ls -l
```

Change owner/group:

```bash
chown user file
chgrp group file
```

---

### **b) Permissions**

Three permission sets → **Owner, Group, Others**.

|Symbol|Meaning|
|---|---|
|**r**|Read|
|**w**|Write|
|**x**|Execute|

Example:

```
-rwxr-x---
```

Interpretation:

- Owner: rwx
- Group: r-x
- Others: ---

Change permissions:

```bash
chmod 755 script.sh
```

### **c) Special Permission Bits**

|Bit|Symbol|Purpose|
|---|---|---|
|**SUID**|s (owner execute)|Program runs with owner's privileges|
|**SGID**|s (group execute)|Inherit group ID of the directory|
|**Sticky bit**|t|Only owner can delete files (commonly in /tmp)|

Example:

```
drwxrwxrwt   /tmp   → sticky bit
```

### **d) Timestamps**

Each file has three key timestamps:

|Timestamp|Meaning|
|---|---|
|**atime**|Last access time|
|**mtime**|Last modification of file content|
|**ctime**|Last metadata change (permissions, owner, etc.)|

Check timestamps:

```bash
stat filename
```

### **e) File Size & Links**

- **Size**: measured in bytes
- **Hard links**: same inode, same data
- **Soft links (symlinks)**: point to another file

Show link count:

```
ls -l   (2nd column shows number of links)
```
