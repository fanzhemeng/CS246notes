# Lecture 05

### Recap 
* `./script <arg1> <arg2>` called be called by `$0`, `$1`, `$2`
* `/dev/null` is the black hole to redirect output we do not care about
* `$?` check status code for last executed command/script
* `$#` total number of arguments to the script

Note: status code `0` if success, non-zero if failed

#### GoodPassword Example 
'''
#!/bin/bash
egrep "^$1$" /usr/share/dict/words > /dev/null
if [ $? -eq 0 ] ; then
		echo "Not a good password"
else 
		echo "Maybe a good password"
fi
'''

#### More commands involving if
* check if a file exists  
  `if [ -e filename ]
* general format for `if` statement:  
  ```
	if [ condition ] ; then 
			...
	elif [ condition ] ; then
			...
	else 
			...
	fi
	```
Note that the whitespaces around condition are immportant!! It is because `[` if 
treated as a command here, and `]` is its last argument, acting like a terminator.

* comparisons
	* integers: `-eq`, `-ne`, `-le`, `-gt`
	* strings: `=`, `!=`
	* logic: `-a` for and, `-o` for or, `!` for not

#### Simple Example 
```
$> [ -e basic ]
$> echo $?
```

```
$> [ 1 -eq 2 ] 
$> echo $?
```
#### More Complicated Example
```
#!/bin/bash

usage () {
		echo "Usage: $0 password"
		exit 1
}

if [ $# -ne 1 ]; then
		usage
fi

egrep "^$1$" /usr/share/dict/words > dev/null

if [ $? -eq 0 ]; then
		echo "Not a good password"
else 
		echo "Maybe a good password"
fi
```

The script above checks if an exact number of arguments are provided.
We define a variable `usage` to improve readability. Usage message is 
commonly isolated into its own function.

### While Loop
#### Example 
Print number from 1 to $1.  
```
#!/bin/bash
x=1
while [ ${x} -le $1 ]; do
		echo ${x}
		x=$((x+1))  # note the two pairs of parentheses here
done
```

Note that it is a common mistake to increment x by doing `x=$x+1`, 
which outputs `1+1` as a string.

### For Loop
####Example 1
Rename all .C files to .cc. The command to rename a file is
`mv Hello.C Hello.cc`, and with consumed arguments, you can do
`mv ${filename} ${filename%C}cc`

```
#!/bin/bash
for filename in *.C; do
		mv ${filename} ${filename%C}cc
done
```

#### Example 2
How many times does $1 occur in $2?

```
count=0
for word in `cat $2`; do
		if [ $word = "$1" ]; then
				count=$((count+1))  # note the double quotes around $1
		fi
done
echo ${count}
```

#### Example
Print the day getting paid of the month, which is the last Firday
of the month. You can use `awk`.

```
echo `cal | awk '{print $6}' | grep "[0-9]" | tail -1`
```

Note the command `cal` will be used in Assignment 1.


