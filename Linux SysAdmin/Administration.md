# **Short Note: Changing Password in Linux**

In Linux, the **`passwd`** command is used to change the password of a user account. A normal user can change **only their own** password, while the root user can change the password of **any account**.

To change your own password:

```bash
passwd
```

The system prompts for:

- Your **current password**
- A **new password** (entered twice)

To change another user’s password (root only):

```bash
passwd username
```

The password is stored securely in encrypted form in `/etc/shadow`. Linux enforces password policies such as minimum length, complexity, and expiration depending on system configuration.

# **Creating and Managing Users in Linux**

Linux allows administrators to create, modify, and manage user accounts to control access and maintain system security. User information is stored in `/etc/passwd`, encrypted passwords in `/etc/shadow`, and group details in `/etc/group`.
## **1. Creating Users**

The `useradd` or `adduser` command is used to create new accounts.

### **Create a new user:**

```bash
sudo useradd username
```

### **Create user with home directory (common):**

```bash
sudo useradd -m username
```

### **Set password for the user:**

```bash
sudo passwd username
```

A home directory is created under `/home/username` and default configuration files are copied.

## **2. Managing Users**

### **a) Modify user details**

Use `usermod`:

|Task|Command|
|---|---|
|Change username|`sudo usermod -l newname oldname`|
|Change home directory|`sudo usermod -d /new/home -m username`|
|Add user to a group|`sudo usermod -aG groupname username`|
|Change login shell|`sudo usermod -s /bin/bash username`|

### **b) Delete users**

```bash
sudo userdel username
```

Delete user **and** home directory:

```bash
sudo userdel -r username
```


### **c) Managing groups**

Groups help assign permissions to multiple users.

Create a group:

```bash
sudo groupadd groupname
```

Add user to a group:

```bash
sudo usermod -aG groupname username
```

Remove a user from a group (edit `/etc/group` or use `gpasswd`):

```bash
sudo gpasswd -d username groupname
```


## **3. Important System Files**

|File|Purpose|
|---|---|
|**/etc/passwd**|Stores basic user information|
|**/etc/shadow**|Stores encrypted passwords|
|**/etc/group**|Stores group information|

