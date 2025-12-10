### **1. Anchors & Boundaries**

These do not match characters; they match _positions_ in the string.

| **Symbol** | **Name**              | **Description**                           | **Example** | **Matches**                             |
| ---------- | --------------------- | ----------------------------------------- | ----------- | --------------------------------------- |
| `^`        | **Start of String**   | Matches the beginning of the text.        | `^Hello`    | "Hello World" (but not "Oh Hello")      |
| `$`        | **End of String**     | Matches the end of the text.              | `end$`      | "The end" (but not "The ending")        |
| `\b`       | **Word Boundary**     | Matches the start or end of a whole word. | `\bwork\b`  | "work" (but not "working" or "network") |
| `\B`       | **Non-Word Boundary** | Matches where `\b` does not.              | `\Bwork`    | "network" (but not "work")              |

### **2. Character Classes**

Shorthand codes for common character types.

Note: Capital letters usually mean the inverse (NOT).

| **Symbol** | **Description**    | **Matches**                                       |
| ---------- | ------------------ | ------------------------------------------------- |
| `.`        | **Any Character**  | Any single character (except newline).            |
| `\d`       | **Digit**          | Any number `0-9`.                                 |
| `\D`       | **Non-Digit**      | Anything that is _not_ a number.                  |
| `\w`       | **Word Character** | Any letter, number, or underscore `[a-zA-Z0-9_]`. |
| `\W`       | **Non-Word Char**  | Symbols, punctuation, spaces.                     |
| `\s`       | **Whitespace**     | Space, tab, newline.                              |
| `\S`       | **Non-Whitespace** | Visible characters.                               |

### **3. Quantifiers**

These determine **how many** of the previous character/group to match. By default, regex is **Greedy** (matches as much as possible).

|**Symbol**|**Name**|**Description**|
|---|---|---|
|`*`|**Star**|**0 or more** times. (Optional, can repeat)|
|`+`|**Plus**|**1 or more** times. (Required at least once)|
|`?`|**Question**|**0 or 1** time. (Optional)|
|`{n}`|**Exact**|Exactly `n` times. (e.g., `{3}`)|
|`{n,}`|**Min**|`n` or more times.|
|`{n,m}`|**Range**|Between `n` and `m` times.|

**Pro Tip (Lazy Matching):** Adding `?` after a quantifier makes it **Lazy** (matches as little as possible).
- `a.*b` matches "a...b...b" (Greedy: matches until the last 'b')
- `a.*?b` matches "a...b" (Lazy: stops at the first 'b')

### **4. Groups & Ranges**

Used to define logic, scope, and specific sets of characters.

|**Symbol**|**Description**|**Example**|**Explanation**|
|---|---|---|---|
|`[]`|**Set**|`[aeiou]`|Matches _any one_ of these vowels.|
|`[^ ]`|**Negated Set**|`[^0-9]`|Matches anything _except_ a digit.|
|`-`|**Range**|`[a-z]`|Matches any lowercase letter.|
|`()`|**Capture Group**|`(abc)+`|Groups "abc" together (matches "abcabc").|
|`|`|**OR Operator**|`cat|
|`(?:)`|**Non-Capturing**|`(?:abc)`|Groups without saving the text for later use.|

### **5. Lookaround (Advanced)**

Used to check what comes before or after a pattern without actually including it in the match result.

- **Positive Lookahead `(?=...)`**: Ensure pattern follows.
    - `\d+(?= dollars)` matches "100" only in "100 dollars".
- **Negative Lookahead `(?!...)`**: Ensure pattern does _not_ follow.
    - `\d+(?! dollars)` matches "100" in "100 pounds" but not in "100 dollars".
- **Positive Lookbehind `(?<=...)`**: Ensure pattern preceded.
    - `(?<=\$)\d+` matches "100" only in "$100".

### **6. Flags (Modifiers)**

These are placed at the end of the regex (e.g., `/pattern/gi`) to change how the engine searches.

- `g` (**Global**): Find all matches, not just the first one.
- `i` (**Case Insensitive**): "A" matches "a".
- `m` (**Multiline**): `^` and `$` match start/end of _lines_, not just the string.

### **7. Common "Cookbook" Patterns**

Copy-paste these for common tasks.

**Email Address**

Code snippet

```
^[\w\.-]+@[\w\.-]+\.\w+$
```

_Simple validation ensuring user@domain.extension format._

**Date (YYYY-MM-DD)**

Code snippet

```
^\d{4}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01])$
```

_Ensures year is 4 digits, month 01-12, day 01-31._

**Strong Password**

Code snippet

```
^(?=.*[A-Z])(?=.*[a-z])(?=.*\d).{8,}$
```

_Enforces: At least 1 uppercase, 1 lowercase, 1 digit, min 8 chars._

**Hex Color Code**

Code snippet

```
^#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$
```

_Matches #FFF or #FFFFFF._

### **How to "Read" a Regex**

When you see a complex regex, break it down from left to right.

**Example:** `^\d{3}-[a-z]+$`

1. `^` : Start of line.
2. `\d{3}` : Exactly 3 digits.
3. `-` : A literal hyphen.
4. `[a-z]+` : One or more lowercase letters.
5. `$` : End of line.

Matches: "123-abc", "999-hello"

 **Fails:** "12-abc", "123-ABC", "123-abc "

