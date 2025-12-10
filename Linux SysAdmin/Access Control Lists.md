
ACL (Access Control List) is an advanced permission mechanism in Linux that provides **more fine-grained control** than traditional file permissions. While standard permissions allow settings only for **owner, group, and others**, ACL enables assigning **specific permissions to individual users or groups**.

ACLs are especially useful in multi-user environments where several users need different levels of access to the same file or directory.

# **1. Purpose of ACL**

ACLs are used when:

- Standard permissions are insufficient
- Multiple users need different access levels
- Flexible and detailed permission management is required

They allow permissions beyond the traditional rwx model.

# **2. Checking ACL Support**

First, ensure the filesystem supports ACL:

```bash
mount | grep acl
```

Most modern Linux systems support ACL by default (ext4, xfs, btrfs).

# **3. Viewing ACL**

Use the `getfacl` command:

```bash
getfacl file.txt
```

Output includes:

- Owner permissions
- Group permissions
- ACL entries for specific users or groups
- The mask (maximum allowed permissions)

Example:

```
user:alice:rwx
group:developers:rw-
mask::rwx
other::r--
```


# **4. Setting ACL**

ACLs are applied using the `setfacl` command.

### **a) Add permission for a user**

```bash
setfacl -m u:alice:rw file.txt
```

### **b) Add permission for a group**

```bash
setfacl -m g:developers:r file.txt
```

### **c) Set default ACL on a directory**

(Default ACLs apply automatically to new files inside the directory.)

```bash
setfacl -m d:u:alice:rwx project/
```

### **d) Recursive ACL application**

```bash
setfacl -R -m g:team:rw shared/
```

# **5. Removing ACL**

### **Remove a specific entry**

```bash
setfacl -x u:alice file.txt
```

### **Remove all ACL entries**

```bash
setfacl -b file.txt
```

# **6. Important ACL Concepts**

### **a) ACL Mask**

The mask defines the **maximum permissions** allowed for:

- Named users
- Groups
- Named groups

If mask is restrictive, ACL entries may not behave as expected.

Set mask explicitly:

```bash
setfacl -m m:rwx file.txt
```

### **b) Default ACL**

Applied only on directories. It ensures newly created files inherit ACL rules.

Example:

```bash
setfacl -m d:g:devteam:rw project/
```

# **7. Advantages of ACL**

- More precise than owner/group/others permission model
- Supports multiple user-specific permissions
- Useful in collaborative environments
- Works seamlessly with existing permission systems

# **8. Limitations of ACL**

- Adds complexity
- Requires careful management of mask
- Not all backup tools preserve ACL unless configured

# **Summary**

ACL (Access Control List) in Linux allows fine-grained access control beyond traditional rwx permissions. Using `getfacl` and `setfacl`, administrators can assign specific permissions to multiple users or groups, apply default ACLs to directories, and manage permissions in a flexible way. ACLs are essential in shared environments where different users require different access levels.