# **1. `cut` Command**

`cut` extracts **specific columns or fields** from lines of text.

### Key Uses

• Extract fields separated by delimiters  
• Select character ranges  
• Process text in structured files (CSV, logs)

### Examples

```bash
cut -d":" -f1 /etc/passwd       # show first field (username)
cut -c1-5 file.txt              # show characters 1 to 5
cut -d"," -f2 students.csv      # second column of CSV
```

# **2. `awk` Command**

`awk` is a **powerful text-processing language**. It works with fields and supports patterns, conditions, and actions.

### Key Uses

• Extract specific columns  
• Perform calculations  
• Format output  
• Filter text based on conditions

### Examples

```bash
awk '{print $1, $3}' file.txt           # print 1st and 3rd fields
awk -F":" '{print $1}' /etc/passwd      # specify delimiter
awk '$3 > 1000' file.txt                # print lines where field 3 > 1000
```

`awk` is especially useful for interpreting logs, reports, and tabular data.

# **3. `grep` and `egrep`**

`grep` searches for **text patterns** in files or output.

### Key Uses

• Find matching words or patterns  
• Filter output in pipelines  
• Highlight text using regex

### Examples

```bash
grep "error" logfile.txt
grep -i "failed" logfile.txt          # case-insensitive
grep -r "keyword" /var/log            # recursive search
```

`egrep` is equivalent to:

```bash
grep -E
```

It supports **extended regular expressions** like `|`, `+`, `?`, and grouping.

Example:

```bash
egrep "cat|dog" file.txt
```


# **4. `sort` Command**

`sort` arranges lines of text **alphabetically or numerically**.

### Key Uses

• Sort text files  
• Sort by specific fields  
• Numeric sorting  
• Reverse sorting

### Examples

```bash
sort names.txt                     # alphabetic sort
sort -r names.txt                  # reverse order
sort -n numbers.txt                # numeric sort
sort -t"," -k2 file.csv            # sort CSV by column 2
```

# **5. `uniq` Command**

`uniq` removes **duplicate lines** from sorted input. It only works properly if input is already sorted.

### Key Uses

• Remove repeated lines  
• Count occurrences  
• Filter unique values

### Examples

```bash
uniq file.txt
uniq -c file.txt     # show counts
sort file.txt | uniq # proper usage
```

# **6. `wc` Command (Word Count)**

`wc` counts **lines**, **words**, and **characters** in a file.

### Key Uses

• Measure file size in terms of text  
• Count lines in logs  
• Check number of words in a document

### Examples

```bash
wc file.txt              # show all counts
wc -l file.txt           # count lines
wc -w notes.txt          # count words
wc -c data.txt           # count characters
```

# **Summary**

|Command|Purpose|
|---|---|
|**cut**|Extract specific columns or characters|
|**awk**|Powerful text processing and scripting|
|**grep / egrep**|Search text using patterns or regex|
|**sort**|Arrange lines alphabetically or numerically|
|**uniq**|Remove duplicate lines, count repeats|
|**wc**|Count lines, words, and characters|

