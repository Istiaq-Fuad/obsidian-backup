# **1. Ownership in Linux**

Every file has two types of owners:

### **a) User (Owner)**

The user who created the file. This person has primary control over the file.

### **b) Group**

A collection of users. Any user belonging to the file’s group receives group-level permissions.

Additionally, “others” refers to all other users on the system.

Thus, permissions are divided into three categories:

1. **Owner**
2. **Group**
3. **Others**

# **2. Types of Permissions**

Each file and directory has three basic permission types:

### **a) Read (r)**

- For files: allows reading the file’s content.
- For directories: allows listing the directory contents.

### **b) Write (w)**

- For files: allows modifying or deleting the file.
- For directories: allows creating, deleting, or renaming files inside.

### **c) Execute (x)**

- For files: allows running the file as a program/script.
- For directories: allows entering (cd into) the directory.

# **3. Viewing Permissions**

Use `ls -l` to display file permissions:

```
-rwxr-x---
```

The structure is:

```
[File type] [Owner perms] [Group perms] [Others perms]
```

Example:

- `rwx` means read, write, execute.
- `r-x` means read and execute.
- `---` means no permission.

# **4. Changing Permissions**

### **a) Symbolic Method**

```bash
chmod u+r file        # add read for owner
chmod g-w file        # remove write from group
chmod o+x script.sh   # add execute for others
chmod u=rwx,g=rx,o=   # set all permissions
```

### **b) Numeric (Octal) Method**

Each permission is represented by a number:

- Read = 4
- Write = 2
- Execute = 1

Add them to form permission codes:

| Permission | Numeric |
| ---------- | ------- |
| rwx        | 7       |
| rw-        | 6       |
| r-x        | 5       |
| r--        | 4       |

Example:

```bash
chmod 755 script.sh   # rwx r-x r-x
chmod 644 file.txt    # rw- r-- r--
```


# **5. Special Permission Bits**

Linux also supports three advanced permission bits for enhanced control:

### **a) SUID (Set User ID)**

Symbol: **s** in owner’s execute field  
Function: A user running the file temporarily gets the file owner's privileges.

Example:

```bash
-rwsr-xr-x
```

### **b) SGID (Set Group ID)**

Symbol: **s** in group’s execute field  
Function:

- On files: runs with group privileges.
- On directories: new files inherit the directory’s group.

Example:

```bash
chmod g+s shared/
```

### **c) Sticky Bit**

Symbol: **t** in others’ execute field  
Function: Only the owner can delete files within that directory.

Common location:

```
drwxrwxrwt /tmp
```

Used for shared directories to prevent unauthorized deletion.


# **6. File Types and Permission Indicators**

The first character in `ls -l` shows the file type:

|Symbol|Meaning|
|---|---|
|-|Regular file|
|d|Directory|
|l|Symbolic link|
|c|Character device|
|b|Block device|

# **7. Why File Permissions Matter**

- They **protect system files** from unauthorized changes.
- Ensure **users’ data privacy**.
- Prevent accidental or malicious execution of scripts.
- Control collaborative environments using group permissions.
- Enhance system stability by restricting access to critical directories.

# **8. `chown` – Change File Ownership**

The `chown` command is used to change the **owner** and/or **group** of a file or directory. Only root or a privileged user can use it.

### **Examples**

```bash
chown user file.txt         # change owner
chown user:group file.txt   # change owner and group
chown :group file.txt       # change group only
chown -R user:group folder/ # recursive ownership change
```

### **Use Cases**

- Assigning files to correct users
- Managing shared directories
- Adjusting ownership after file transfers

# **9. `chgrp` – Change Group Ownership**

`chgrp` changes only the **group** associated with a file or directory.

### **Examples**

```bash
chgrp staff file.txt
chgrp -R developers project/
```

### **Use Cases**

- Allow groups to collaborate on shared files
- Control read/write access for team members

# **10. How These Commands Work Together**

A typical Linux file has:

- **Owner**
- **Group**
- **Permissions**

Example workflow:

```bash
chown alice:developers project.txt   # set owner and group
chmod 660 project.txt                # owner & group can read/write
```

This setup allows only Alice and members of "developers" to modify the file.

# **11. Related Commands**

### **a) `umask`**

Controls **default permissions** for new files and directories.

Example:

```bash
umask 022
```

### **b) `stat`**

Displays detailed information about permissions and ownership.

Example:

```bash
stat file.txt
```

### **c) `id`**

Shows user and group IDs to verify permissions.


# **6. Importance of Permission and Ownership Commands**

These commands help:

- Secure sensitive data
    
- Prevent unauthorized access
    
- Enable controlled sharing of files
    
- Ensure proper file management in multi-user environments
    

They form the foundation of Linux system security.

---

# **Summary**

- **`chmod`** changes read, write, and execute permissions (numeric or symbolic).
- **`chown`** changes file owner and group, usually by root.
- **`chgrp`** changes only the group of a file or directory.  
    Together, these commands give administrators fine-grained control over file access and security in Linux.