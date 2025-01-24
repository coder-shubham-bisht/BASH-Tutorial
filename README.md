# BASH-Tutorial


# Shell Scripting Guide

This document provides an overview of shell functions and parameters, including positional and special parameters.

---

##  **Shell Functions**

Shell functions are reusable blocks of code within a shell script.

They simplify scripting by avoiding redundancy and improving maintainability.

### **Defining a Function**
```bash
function_name() {
    # Commands go here
}
```
OR
```bash
function function_name {
    # Commands go here
}
```

### **Calling a Function**
```bash
function_name
```

### **Example**
```bash
greet() {
    echo "Hello, $1!"
}

greet "World"  # Output: Hello, World!
```

### **Features of Shell Functions**

- **Local Variables:** Variables scoped to the function using `local`.
  ```bash
  my_function() {
      local var="local value"
      echo $var
  }
  ```

- **Returning Values:** Return status codes (0–255) using `return`.
- to return something as stdout use echo command
  ```bash
  my_function() {
      return 42
  }
  my_function
  echo $?  # Output: 42
  ```

  ```bash
  my_function() {
      echo 42
  }
  my_function
  terminal # Output: 42
  ```
---

## **Shell Parameters**

Shell parameters are variables used by the shell to store data passed to scripts or functions. These include **positional parameters** and **special parameters**.

### **Positional Parameters**

Positional parameters are arguments passed to the script or function during execution. They are denoted as `$1`, `$2`, `$3`, ..., `$9`. For arguments beyond `$9`, use braces: `${10}`.

#### **Example**
```bash
#!/bin/bash
echo "First argument: $1"
echo "Second argument: $2"
```
Run it:
```bash
./script.sh arg1 arg2
# Output:
# First argument: arg1
# Second argument: arg2
```

#### **Shifting Positional Parameters**
The `shift` command moves positional parameters left by one, discarding `$1` and reassigning others.
```bash
#!/bin/bash
echo "Before shift: $1 $2"
shift
echo "After shift: $1 $2"
```

---

### **3.4.2 Special Parameters**

Special parameters are predefined variables with specific meanings:

| Parameter | Description                                                                                     |
|-----------|-------------------------------------------------------------------------------------------------|
| `$0`      | The name of the script or shell.                                                               |
| `$#`      | The number of positional parameters.                                                           |
| `$*`      | All positional parameters as a single string (quoted as one).                                  |
| `"$*"`    | All positional parameters as a single quoted string (treated as one argument).                 |
| `$@`      | All positional parameters as separate arguments.                                               |
| `"$@"`    | All positional parameters as separate quoted strings (treated as separate arguments).          |
| `$?`      | The exit status of the last executed command.                                                  |
| `$$`      | The Process ID (PID) of the current shell or script.                                           |
| `$!`      | The PID of the last background process.                                                        |
| `$_`      | The last argument of the last command.                                                         |

#### **Examples**
```bash
#!/bin/bash
echo "Script name: $0"
echo "Number of arguments: $#"
echo "Arguments as a string: $*"
echo "Arguments as separate strings: $@"
echo "Last command status: $?"
echo "PID of this script: $$"
```

---
## **Shell Expansions**

Shell expansions allow for advanced processing of strings, variables, commands, and patterns in shell scripts.

It Allows the shell to interpret, process, and transform certain patterns, commands, or variables into more complex forms before execution. 
enabling dynamic content generation.

### **Brace Expansion**
Brace expansion generates a sequence of strings or patterns.

#### **Example**
```bash
echo {1..5}   # Output: 1 2 3 4 5
echo {a..e}   # Output: a b c d e
```

### **Tilde Expansion**
The tilde (`~`) is expanded to the home directory of the current or specified user.

#### **Example**
```bash
echo ~        # Output: /home/username
echo ~root    # Output: /root
```

### **Shell Parameter Expansion**
Parameter expansion retrieves and modifies variable values.

#### **Example**
```bash
var="Hello"
echo ${var}           # Output: Hello
echo ${var:0:2}      # Output: He (substring)
echo ${var:-Default} # Output: Hello
```

### **Command Substitution**
Command substitution captures the output of a command.

#### **Example**
```bash
now=$(date)
echo "Current date and time: $now"
```

### **Arithmetic Expansion**
Performs arithmetic operations.

#### **Example**
```bash
echo $((5 + 3))  # Output: 8
echo $((2 * 4))  # Output: 8
```

### **Process Substitution**
Allows a command's output to be treated as a file.

#### **Example**
```bash
diff <(ls dir1) <(ls dir2)

read -r var1 var2 < <(echo "apple banana")
echo "var1: $var1, var2: $var2"

```


### **Word Splitting**
Splits a string into words based on the Internal Field Separator (IFS).
**The Role of IFS in Word Splitting:**
Word splitting in Bash happens when you assign a string to a variable or use it in a command, and this process is controlled by the Internal Field Separator (IFS).
When you set IFS=":", you're telling Bash to treat the colon (:) as the delimiter that separates words in a string.
Internal Field Separator (IFS).
#### **Example**
```bash
IFS=":"
str="word1:word2:word3"
for word in $str; do
    echo $word
done
```

### **

Filename Expansion**
Uses wildcards to match filenames.

#### **Example**
```bash
echo *.txt   # Outputs all .txt files in the directory
```

#### **3.5.8.1 Pattern Matching**
Advanced matching with wildcards and patterns.

| Pattern   | Description                    |
|-----------|--------------------------------|
| `*`       | Matches any string.            |
| `?`       | Matches a single character.    |
| `[abc]`   | Matches any character in set.  |
| `[a-z]`   | Matches any character in range.|

### **3.5.9 Quote Removal**
Removes quotes from a string.

#### **Example**
```bash
echo "Quoted string"   # Output: Quoted string
echo 'Another string'  # Output: Another string
```

---



# Bash `for` Loop - In-depth Explanation
---

## Syntax of the `for` Loop

The general syntax of a `for` loop in Bash is:

```bash
for variable in list; do
    # Commands to execute
done
```

### Components:
- **`variable`**: This is a temporary variable that takes the value of each element in the `list` during each iteration.
- **`list`**: This is the sequence of items over which the loop will iterate. The list can be a range, a list of items, or the output of a command.
- **`do`**: Marks the beginning of the block of commands that will be executed in each iteration.
- **`done`**: Marks the end of the loop.

---

## Types of `for` Loops

There are different types of `for` loops in Bash depending on the use case.

### **For Loop Over a List**

This type of loop iterates over a set of items (words, numbers, or files). Each item is processed in sequence.

```bash
for item in apple banana cherry; do
    echo "Fruit: $item"
done
```

### Output:
```
Fruit: apple
Fruit: banana
Fruit: cherry
```

#### Explanation:
- The loop iterates over each item (`apple`, `banana`, `cherry`).
- In each iteration, the value of `item` changes and the `echo` statement prints the current fruit.

---

### **For Loop Over a Range of Numbers**

You can use the `for` loop to iterate over a range of numbers using brace expansion or the `seq` command.

#### Using Brace Expansion:
```bash
for i in {1..5}; do
    echo "Number: $i"
done
```

#### Output:
```
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

#### Explanation:
- The loop iterates over the numbers 1 through 5.
- The value of `i` changes during each iteration, and `echo` prints the current number.

#### Using `seq` Command:
```bash
for i in $(seq 1 5); do
    echo "Number: $i"
done
```

This also produces the same output as the brace expansion example.

---

### **For Loop Over the Output of a Command**

You can use the `for` loop to iterate over the output of a command. This is a common use case, where you want to process a list of files, directories, or any other output from a command.

```bash
for file in $(ls); do
    echo "File: $file"
done
```

#### Explanation:
- The command `ls` generates a list of files, which is then processed by the `for` loop.
- Each filename is stored in the `file` variable, and `echo` prints it.

---

## Word Splitting and IFS (Internal Field Separator)

### Word Splitting:
In Bash, when you iterate over a string or command output using a `for` loop, the string is split into individual words (or tokens) based on the **Internal Field Separator (IFS)**. The default IFS is space, tab, and newline.

#### Example:
```bash
IFS=":"  # Set IFS to colon ":"
str="word1:word2:word3"
for word in $str; do
    echo $word
done
```

#### Output:
```
word1
word2
word3
```

- **How the splitting works**: The string `str="word1:word2:word3"` is split at the colons (`:`) because IFS is set to `:`.
- Each word is processed in a separate iteration of the loop.

### Modifying IFS:
You can modify the IFS temporarily within a loop to control how a string is split.

```bash
IFS=":"  # Temporarily set IFS to colon
for word in $str; do
    echo "Word: $word"
done
IFS=" "  # Reset IFS to space after the loop
```

---

## Using `continue` and `break` in the `for` Loop

### 1. **`continue`**:
The `continue` statement is used to skip the current iteration and move to the next one.

#### Example:
```bash
for i in {1..5}; do
    if [ $i -eq 3 ]; then
        continue
    fi
    echo "Number: $i"
done
```

#### Output:
```
Number: 1
Number: 2
Number: 4
Number: 5
```

- The loop skips printing `Number: 3` because of the `continue` statement.

### 2. **`break`**:
The `break` statement is used to exit the loop entirely.

#### Example:
```bash
for i in {1..5}; do
    if [ $i -eq 3 ]; then
        break
    fi
    echo "Number: $i"
done
```

#### Output:
```
Number: 1
Number: 2
```

- The loop stops completely when `i` reaches 3 due to the `break` statement.

---

## Nested `for` Loops

You can nest `for` loops to iterate over multiple sequences. This is useful for iterating over two-dimensional structures like arrays or files within directories.

#### Example:
```bash
for i in {1..2}; do
    for j in {a..c}; do
        echo "Pair: $i$j"
    done
done
```

#### Output:
```
Pair: 1a
Pair: 1b
Pair: 1c
Pair: 2a
Pair: 2b
Pair: 2c
```

- The outer loop iterates over `{1..2}`, and the inner loop iterates over `{a..c}`.
- Each pair of `i` and `j` is printed.

---

## Special Considerations

1. **Command Substitution**: When using command substitution (`$(command)`), the output of the command is split into words based on the IFS. This can lead to unexpected behavior if the output contains spaces or other special characters.
   - Always be mindful of IFS settings when using command substitution inside loops.

2. **Quoting Variables**: When iterating over variables that may contain spaces, always quote the variable to prevent word splitting from occurring.
   
   ```bash
   for file in "$str"; do
       echo "$file"
   done
   ```

---


### **`if` Statement**
The `if` statement is used to execute a block of code only if a specified condition is true. 

#### Syntax:
```bash
if condition; then
    # commands to execute if condition is true
fi
```

- `condition`: An expression that evaluates to either true (exit status 0) or false (non-zero exit status).
- `then`: If the condition evaluates to true, the block of commands after `then` will execute.
- `fi`: Closes the `if` block.

#### Example:
```bash
if [ -f "file.txt" ]; then
    echo "file.txt exists."
fi
```
This checks if `file.txt` exists as a regular file, and if true, it prints the message.

### 2. **`else` Statement**
The `else` statement is used in conjunction with `if` to execute a block of code when the condition is false.

#### Syntax:
```bash
if condition; then
    # commands to execute if condition is true
else
    # commands to execute if condition is false
fi
```

#### Example:
```bash
if [ -f "file.txt" ]; then
    echo "file.txt exists."
else
    echo "file.txt does not exist."
fi
```

### **`elif` (Else If) Statement**
The `elif` statement provides an alternative condition to test if the `if` condition is false. You can have multiple `elif` statements in a single `if` block.

#### Syntax:
```bash
if condition1; then
    # commands to execute if condition1 is true
elif condition2; then
    # commands to execute if condition2 is true
else
    # commands to execute if neither condition is true
fi
```

#### Example:
```bash
if [ -f "file.txt" ]; then
    echo "file.txt exists."
elif [ -f "otherfile.txt" ]; then
    echo "otherfile.txt exists."
else
    echo "Neither file.txt nor otherfile.txt exists."
fi
```

### **Test Conditions**
The `test` command in Bash (often used with square brackets `[ ]`) is used to evaluate conditional expressions. The `test` command checks various types of conditions, such as file existence, string comparisons, and numerical comparisons. When combined with the `if` statement, `test` allows scripts to make decisions based on these conditions.

### 1. **The `test` Command**
The `test` command checks conditions and returns an exit status based on whether the condition is true or false:
- `0` means true (success).
- Non-zero means false (failure).

It is commonly used with square brackets `[` and `]`, which is equivalent to calling `test`.

#### Syntax:
```bash
test condition
# or equivalently
[ condition ]
```

### 2. **Common Conditions Used with `test`**

#### **File Conditions**
These check various file properties:
- `-f file`: True if `file` exists and is a regular file.
- `-d file`: True if `file` exists and is a directory.
- `-e file`: True if `file` exists (of any type).
- `-r file`: True if `file` exists and is readable.
- `-w file`: True if `file` exists and is writable.
- `-x file`: True if `file` exists and is executable.
- `-s file`: True if `file` exists and is not empty.

#### Example:
```bash
if [ -f "file.txt" ]; then
    echo "file.txt exists and is a regular file."
else
    echo "file.txt does not exist or is not a regular file."
fi
```

#### **String Conditions**
These check string properties:
- `-z string`: True if the string is empty.
- `-n string`: True if the string is not empty.
- `string1 == string2`: True if the strings are equal.
- `string1 != string2`: True if the strings are not equal.

#### Example:
```bash
str="Hello"
if [ -n "$str" ]; then
    echo "The string is not empty."
else
    echo "The string is empty."
fi
```

#### **Numerical Conditions**
These check numerical values:
- `num1 -eq num2`: True if `num1` is equal to `num2`.
- `num1 -ne num2`: True if `num1` is not equal to `num2`.
- `num1 -lt num2`: True if `num1` is less than `num2`.
- `num1 -le num2`: True if `num1` is less than or equal to `num2`.
- `num1 -gt num2`: True if `num1` is greater than `num2`.
- `num1 -ge num2`: True if `num1` is greater than or equal to `num2`.

#### Example:
```bash
num1=5
num2=10
if [ "$num1" -lt "$num2" ]; then
    echo "$num1 is less than $num2."
else
    echo "$num1 is not less than $num2."
fi
```

### 3. **The `if` Statement with `test` Command**
The `test` command is frequently used inside `if` statements to check conditions. If the condition in the `test` command evaluates to true (i.e., exit status 0), the `then` block will be executed. If the condition evaluates to false (non-zero exit status), the `else` block (if present) will execute.

#### Syntax:
```bash
if [ condition ]; then
    # commands to execute if condition is true
else
    # commands to execute if condition is false
fi
```

### 4. **Examples of Using `test` with `if`**

#### **File Check Example:**
```bash
if [ -e "file.txt" ]; then
    echo "file.txt exists."
else
    echo "file.txt does not exist."
fi
```
- This checks if `file.txt` exists (of any type). If it does, the first block executes; otherwise, the second block executes.

#### **String Comparison Example:**
```bash
str1="Hello"
str2="World"
if [ "$str1" == "$str2" ]; then
    echo "The strings are equal."
else
    echo "The strings are not equal."
fi
```
- This compares two strings. If they are equal, it prints a message; otherwise, it prints a different message.

#### **Numerical Comparison Example:**
```bash
num1=100
num2=50
if [ "$num1" -gt "$num2" ]; then
    echo "$num1 is greater than $num2."
else
    echo "$num1 is not greater than $num2."
fi
```
- This compares two numbers and executes the appropriate block depending on the result.

### 5. **Logical Operators with `test` and `if`**

You can combine multiple conditions using logical operators in `test` conditions:

- `&&` (AND): Both conditions must be true.
- `||` (OR): At least one condition must be true.

#### Example with AND (`&&`):
```bash
if [ -f "file1.txt" ] && [ -f "file2.txt" ]; then
    echo "Both file1.txt and file2.txt exist."
else
    echo "At least one of the files does not exist."
fi
```

#### Example with OR (`||`):
```bash
if [ -f "file1.txt" ] || [ -f "file2.txt" ]; then
    echo "At least one of the files exists."
else
    echo "Neither file1.txt nor file2.txt exists."
fi
```

### 6. **Using `[[ ]]` (Improved Test Command)**
While `[ ]` is the standard way to test conditions in Bash, `[[ ]]` (the extended test command) is more powerful and provides more flexibility, especially when dealing with complex conditions, such as pattern matching.

- `[[ ... ]]` allows the use of operators like `&&` and `||` without the need for escaping.
- It supports regular expression matching (`=~`).
- It handles word splitting and filename expansion more safely than `[ ]`.

#### Example with `[[ ]]`:
```bash
str="hello"
if [[ "$str" == "hello" ]]; then
    echo "The string is hello."
else
    echo "The string is not hello."
fi
```

#### Example with Regular Expression Matching:
```bash
str="hello123"
if [[ "$str" =~ ^hello ]]; then
    echo "The string starts with 'hello'."
else
    echo "The string does not start with 'hello'."
fi
```

### 7. **Exit Status of `test` and `if`**
- The `test` command returns an exit status, where `0` means true (success) and non-zero means false (failure).
- The `if` statement evaluates the exit status of the command it runs. If the exit status is `0`, the `then` block executes. If non-zero, the `else` block (if present) executes.

#### Example:
```bash
if test -f "file.txt"; then
    echo "file

#### - Using `[[ ]]` (Extended Test command)
The `[[ ]]` is a more modern and flexible syntax for conditional tests in Bash. It allows for more complex conditions, such as pattern matching.

Example:
```bash
if [[ "$str" == "hello" ]]; then
    echo "The string is hello."
fi
```

### **Logical Operators in Conditionals**
You can combine multiple conditions using logical operators within `if`, `elif`, or `while` blocks.

- `&&` (AND): Both conditions must be true.
- `||` (OR): At least one condition must be true.

#### Example:
```bash
if [ -f "file.txt" ] && [ -f "otherfile.txt" ]; then
    echo "Both files exist."
fi

if [ -f "file.txt" ] || [ -f "otherfile.txt" ]; then
    echo "At least one file exists."
fi
```

### **Nested Conditionals**
You can nest `if` statements within each other to check multiple levels of conditions.

#### Example:
```bash
if [ -f "file.txt" ]; then
    if [ -s "file.txt" ]; then
        echo "file.txt exists and is not empty."
    else
        echo "file.txt exists but is empty."
    fi
else
    echo "file.txt does not exist."
fi
```

### 7. **`case` Statement**
The `case` statement allows for multiple conditions based on pattern matching. It’s similar to a switch-case construct in other programming languages.

#### Syntax:
```bash
case "$variable" in
    pattern1)
        # commands to execute if pattern1 matches
        ;;
    pattern2)
        # commands to execute if pattern2 matches
        ;;
    *)
        # default commands if no pattern matches
        ;;
esac
```

#### Example:
```bash
read -p "Enter a number between 1 and 3: " num
case "$num" in
    1)
        echo "You chose number 1."
        ;;
    2)
        echo "You chose number 2."
        ;;
    3)
        echo "You chose number 3."
        ;;
    *)
        echo "Invalid choice."
        ;;
esac
```

### **Exit Status**
The exit status of a command or test condition is important for Bash conditionals. A command returns an exit status that indicates success (0) or failure (non-zero). Conditional statements evaluate these exit statuses.

- `0` means success (true).
- Any non-zero value means failure (false).

Example:
```bash
if some_command; then
    echo "Command succeeded."
else
    echo "Command failed."
fi
```

### **`return` vs `exit`**
- `return` is used within functions to exit from a function and optionally return a status code.
- `exit` is used to terminate the script with a specified exit status code.

#### Example with `return`:
```bash
my_function() {
    if [ "$1" -gt 0 ]; then
        return 0  # Successful exit status
    else
        return 1  # Failure exit status
    fi
}
my_function 5
```

