# **1. `tar` – Archive and Extract Files**

`tar` (tape archive) is the most commonly used tool for bundling files into a **single archive file**. It can also compress archives using gzip or bzip2.

### **Common Tar Options**

- `c` = create
- `x` = extract
- `v` = verbose
- `f` = file name
- `z` = use gzip
- `j` = use bzip2
- `J` = use xz
### **Create a tar archive**

```bash
tar -cf files.tar folder/
```

### **Create a compressed tar.gz archive**

```bash
tar -czf files.tar.gz folder/
```

### **Extract a tar archive**

```bash
tar -xf files.tar
```

### **Extract a tar.gz archive**

```bash
tar -xzf files.tar.gz
```

### **List contents without extracting**

```bash
tar -tf files.tar.gz
```

Tar is used when you want **multiple files in one compressed archive**.

# **2. `gzip` – Compress and Decompress Files**

`gzip` compresses individual files using the `.gz` format.

### **Compress a file**

```bash
gzip file.txt
```

This replaces `file.txt` with `file.txt.gz`.

### **Decompress a file**

```bash
gunzip file.txt.gz
```

Or:

```bash
gzip -d file.txt.gz
```

### **Compress without deleting original**

```bash
gzip -c file.txt > file.txt.gz
```

`gzip` is widely used for fast, efficient compression.

# **3. `bzip2` – High-Compression Tool**

Provides **better compression than gzip**, but slower.

### **Compress**

```bash
bzip2 file.txt
```

### **Decompress**

```bash
bunzip2 file.txt.bz2
```

Or:

```bash
bzip2 -d file.txt.bz2
```

### **Tar with bzip2**

```bash
tar -cjf files.tar.bz2 folder/
tar -xjf files.tar.bz2
```

Best used when you want **higher compression ratio**.

# **4. `xz` – Very High Compression**

`xz` offers even better compression than bzip2.

### **Compress**

```bash
xz file.txt
```

### **Decompress**

```bash
xz -d file.txt.xz
```

### **Tar with xz**

```bash
tar -cJf files.tar.xz folder/
tar -xJf files.tar.xz
```

Used when maximum compression is preferred, even if slower.

# **5. `zip` and `unzip` – Windows-Compatible Compression**

`zip` creates compressed archives similar to Windows `.zip` files. It is commonly used for sharing files across operating systems.

### **Create a zip file**

```bash
zip archive.zip file1 file2
```

### **Zip a folder**

```bash
zip -r archive.zip folder/
```

### **Extract zip file**

```bash
unzip archive.zip
```

Useful for portability between Linux, Windows, and MacOS.

# **6. Choosing the Right Tool**

|Tool|Purpose|Compression Level|Multi-File Support|
|---|---|---|---|
|**tar**|Archive files together|None (by default)|Yes|
|**tar.gz**|Fast compression|Medium|Yes|
|**tar.bz2**|Higher compression|High|Yes|
|**tar.xz**|Best compression|Very high|Yes|
|**gzip**|Quick file compression|Medium|No|
|**bzip2**|Better compression|High|No|
|**xz**|Strongest compression|Very high|No|
|**zip**|Cross-platform compatibility|Medium|Yes|

