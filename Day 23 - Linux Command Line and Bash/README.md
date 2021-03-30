Resources Used : https://tryhackme.com/room/linuxmodules

# Basic Syntax :

Declaring Variables:

```bash
#!/usr/bin/env bash

NAME="John"
echo "Hello $NAME!"
echo $NAME
echo "$NAME"
echo "${NAME}!"
```

Functions :

```bash
# One Way
myfunc() {
    echo "hello $1"
}
# Second Way
function myfunc() {
    echo "hello $1"
}
# Calling it

myfunc "John"
```

```bash
# Declaring local variables in Functions
myfunc() {
    local myresult='some value'
    echo $myresult
}
```

Raise Errors :

```bash
myfunc() {
  return 1
}
if myfunc; then
  echo "success"
else
  echo "failure"
fi
```

Defining Arrays :

```bash
Fruits=('Apple' 'Banana' 'Orange')

Fruits[0]="Apple"
Fruits[1]="Banana"
Fruits[2]="Orange"
```

Operations on these Arrays :

```bash
Fruits=("${Fruits[@]}" "Watermelon")    # Push
Fruits+=('Watermelon')                  # Also Push
Fruits=( ${Fruits[@]/Ap*/} )            # Remove by regex match
unset Fruits[2]                         # Remove one item
Fruits=("${Fruits[@]}")                 # Duplicate
Fruits=("${Fruits[@]}" "${Veggies[@]}") # Concatenate
lines=(`cat "logfile"`)                 # Read from file
echo ${Fruits[0]}           # Element #0
echo ${Fruits[-1]}          # Last element
echo ${Fruits[@]}           # All elements, space-separated
echo ${#Fruits[@]}          # Number of elements
echo ${#Fruits}             # String length of the 1st element
echo ${#Fruits[3]}          # String length of the Nth element
echo ${Fruits[@]:3:2}       # Range (from position 3, length 2)
echo ${!Fruits[@]}          # Keys of all elements, space-separated
```

Iteration over an Array :

```bash
for i in "${arrayName[@]}"; do
  echo $i
done
```

If loops :

```bash
# Basic Loop
for i in /etc/rc.*; do
  echo $i
done
# Loop like C
for ((i = 0 ; i < 100 ; i++)); do
  echo $i
done
# With Ranges
for i in {1..5}; do
    echo "Welcome $i"
done
```

Passing Positional Arguments :

```bash
#!/bin/bash

# 2 get positional arguements we can use $1 like so
# Also string concatenation works mostly with " "
echo "Hello there! $1"
```

Setting Default Values :

```bash
#!/bin/bash

name=${1:-"Something"}
echo "My name is $name."
```

This will set the default value of name as something if there isn't an argument passed with the script.

To pop up a error if the $1 argument is not passed we can do this :

```bash
#!/bin/bash

name=${1:?"Yo asked your name mahn?"}
echo "My name is $name."
```

Pass errors if more arguments then needed are passed :

```bash
if [ $# -ne 1 ]; then
        echo "Usage: Hello Hello Hello"
        exit 1
    fi
echo "We good Homie, $1"
exit 1
```

### Some tools that help make life a little better for us

du : This command helps us see the disk usage , and shows which dir uses how much space. Size is shown in kb. Normally if run without any flags like so :

```bash
du # Shows disk usage of all directories in the current dir
du -a # Shows disk usage of files as well as directories
du -h # Lists file sizes in KB,GB,MB,B
du -d No.ofDepth # To specify the depth
```

grep : This is used to filter search a file for a certain pattern in them and displays the line where that pattern is found/matched.

Basic Syntax goes something like this :

```bash
grep "PATTERN" file.txt
```

The patterns could be of two type :

1. Find stuff using a regex

```bash
grep -E "regex" file.txt
# or we can use egrep
egrep "regex" file.txt
# Example :
grep -E "([1-9])\w+" grep.txt # would match numbers 1-9 and any words after it
```

2. Find stuff using a fixed string inside the text :

```bash
grep -F "Plus Ultra" file.txt # Use it to match a sentence that contains Plus Ultra
# or we can use fgrep
fgrep "string" file.txt
```

You can then use flags like :
-R : recursive grep on the files inside the folders, by default its not recursive.
-h : Hides the file names if you are doing recursive sarching
-i : ignores the case of the value (Doesnt care if its lowercase or uppercase )

You can use `man grep` to get a manual for grep and for more stuff it can be used for

Also to make/use/test you can see this website : https://regexr.com/

tr : This is translate command, it can help us in a lot of String Operators, like Changing character cases in a string to replace characters in a string.

Syntax :

```
tr [flag] [source]/[find]/[select] [destination]/[replace]/[change]
```

Some of the flags are :
-d : Delete a given set of characters
-t : concat source set with a destination set
-s : replace the source set with the destination set
-c : This is a reverse so basically you filter will apply on the thing that doesnt match the filter

Examples :

```bash
cat file.txt | tr -d '[a-cA-C= ]' # If you use this in tr.txt from test file it should print youdidit
# Should turn all the content of newtr.txt to lowercase
cat newtr.txt| tr -s '[:upper:]' '[:lower:]'
```

AWK : This is an all in one tool and is basically a sctipting languaged used to manipulate data and generating reports.

Syntax : `awk [flags] [select patter/find(sort)/commands] [input file]`
P.S : You can also pipe output

you can also write a script and pass it like so :

```bash
awk -f script.awk file.txt
```

Now lets get to the basic usage of this tool :

```bash
awk '{print}' file.txt # To Print the text as it is
awk '/pattern/' file.txt # The pattern here is enclosed inside the / / and then its matched against file.txt
```

uniq : This filters the output and removes any duplicates, so we get all the unique stuff.

```bash
cat uniq.txt | uniq
```

sort : This basically sorts the lines alphabetically and numeracally.

```bash
cat sort.txt | sort
```
