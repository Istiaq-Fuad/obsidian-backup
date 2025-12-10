# **Note on Basic Linux File and Directory Commands**

Linux provides essential commands for creating, viewing, copying, moving, and deleting files and directories. These commands help users manage the file system efficiently.

# **1. File-Related Commands**

## **a) `cp` — Copy Files**

Copies files or directories.

```bash
cp file1 file2
cp -r dir1 dir2   # recursive copy for directories
```

## **b) `mv` — Move or Rename Files**

Used to move or rename files and folders.

```bash
mv file.txt /home/user/
mv oldname newname
```

## **c) `rm` — Remove/Delete Files**

Deletes files or directories permanently.

```bash
rm file.txt
rm -r folder/     # remove directory recursively
rm -i file.txt    # prompt before delete
```

## **d) `touch` — Create Empty Files / Update Timestamp**

```bash
touch newfile.txt
```

## **e) `cat` — View File Contents**

Displays the content of text files.

```bash
cat file.txt
```

## **f) `echo` — Print Text or Variables**

Used for displaying text or writing to files.

```bash
echo "Hello"       # show text
echo "data" >> f.txt   # append to file
```

## **g) `rename` — Bulk Rename Files**

Renames many files using patterns.

```bash
rename 's/.txt/.bak/' *.txt
```


# **2. Directory-Related Commands**

## **a) `mkdir` — Create Directories**

```bash
mkdir newfolder
mkdir -p a/b/c     # create nested folders
```

## **b) `rmdir` — Remove Empty Directory**

```bash
rmdir emptyfolder
```

## **c) `ls` — List Files and Directories**

```bash
ls
ls -l      # long listing
ls -a      # show hidden files
```

## **d) `cd` — Change Directory**

```bash
cd /home/user/
cd ..      # go up one directory
cd ~       # go to home directory
```

## **e) `pwd` — Show Current Directory**

```bash
pwd
```

## **f) `du` — Show Disk Usage of Files/Directories**

```bash
du -h folder
```

## **g) `df` — Show File System Disk Space**

```bash
df -h
```


# **Wildcards in Linux**

Wildcards are special characters used in Linux to **match patterns** in filenames and commands. They allow users to work with multiple files at once without typing each name. Wildcards are commonly used with commands like `ls`, `cp`, `mv`, `rm`, `grep`, and `find`.

## **Types of Wildcards**

### **1. Asterisk (*)**

Matches **zero or more characters**.

Examples:

```bash
ls *.txt          # all .txt files
rm file*          # files starting with 'file'
cp * /backup      # all files in the current directory
```

### **2. Question Mark (?)**

Matches **exactly one character**.

Examples:

```bash
ls file?.txt      # matches file1.txt, fileA.txt but not file10.txt
mv img?.png test/ # files like img1.png, img2.png
```

### **3. Square Brackets ([])**

Matches **any one character** from a set or range.

Examples:

```bash
ls file[123].txt      # matches file1.txt, file2.txt, file3.txt
ls report[aeiou].pdf  # matches files ending with a vowel
ls img[0-9].jpg       # numeric range
```

### **4. Negation inside brackets [! ]**

Matches any character **NOT** in the set.

Example:

```bash
ls file[!0-9].txt     # files not ending with a number
```

### **5. Brace Expansion ({ })**

Creates **specific patterns** (not true wildcards, but often grouped with them).

Examples:

```bash
cp file.{txt,pdf} /backup
mv image_{1..5}.jpg pics/
```

## **Use of Wildcards with Commands**

Wildcards help perform bulk operations:

```bash
rm *.log                   # delete all .log files
cp report??.pdf reports/   # files with two characters after 'report'
grep "error" *.conf        # search in all .conf files
```

## **Advanced Use with Find**

Find supports pattern matching with wildcards:

```bash
find . -name "*.sh"
find /var/log -name "syslog?.*"
```
