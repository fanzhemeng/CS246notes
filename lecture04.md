# Lecture 04

## Recap: Regular expression special characteristics
* `.` -- match any one character
* `?` -- 0 or 1 of prceding sub-pattern
* `+` -- 1 or more 
* `*` -- 0 or more
* `^` -- start with
* `$` -- end with

to copy a1.pdf to your local machine: `$> scp userid@linux.student.cs.uwaterloo.ca: cs246/1155/a1/a1.pdf .`

#### Example 1 
Print all words in usr/share/dict/words that are even length.

`$> egrep "^(..)*$" /usr/share/dict/words `

#### Example 2 
Print all words in the dict that start with 'e' and consist of 5 chars.

`$> egerp "^e....$" /usr/share/dict/words `

#### Example 3 
Print all filenames in the current dir whose names contain exactly one 'a'.

`$> ls | egrep "^[^a]*a[^a]*$" `

## File Permissions
Commands: `$> ls -l `     // long listing

Output: `-rwxrw-r-- 1 nanaeem staff 28 23 Feb 2015 08:24:38 fname.txt`

* File type (the first column)
	* `-` -- ordinary files
	* `d` -- directories
	* `l` -- symbolic links

* Bits
	* Three kinds -- user bits, froup bits, and other bits
		* user bits: owner permission
		* group bits: other users in the group 
		* other bits: all other users
	* Three options:
		* `r` - read
		* `w` - write
		* `x` - execute

* Other info by `ls -l`
	* number of symbolic links
	* owner name
	* group name
	* size 
	* last modified
	* filename

### How to change file permissions

command: `$> chmod mode file `

* mode
	* ownership: 
	  `u` for user, `g` for group, `o` for others, `a` for all
	* operator: 
	  `+` for add, `-` for reduce, `=` for set to
	* permission: 
	  `r`, `w`, `x`

Note that only owner can change the permissions. (but copying changes ownersip, should read more about copy)

#### Example 1
Give others read permission.

`$> chmod 0+r file `

#### Example 2
Revoke execute permission from group

`$> chmod g-x file `

#### Example 3
Make everyone permission rw-.

`$> chmod a=rw file `

Alternatively, we can do `$> chmod 666 file `, and note that 744 is common.

## Shell Variables
### define a variable
`$> x=1`  (note that there is no whitespaces)  
`$> echo $x`  
Then output: `1`

Note:   
1.do not use `$` when setting variables  
2.use `$` to access the value of the variable  
3.no space in variable definition  

We can also set a variable as a directory path:   
`$> scdir=~/user/nanaeem/1155`   
`$> ls $scdir`  
Here, note that `$> echo "$scdir"` show the value of scdir, while `$> echo '$scdir'` 
see $scdir as string.

#### Example  
``$> x=`pwd```  
`$> cd ..`  
`$> echo $x`  
The output of the above commands indicates x is a variable. That is, once set, it wont 
change unless you change it.

## Shell Script
Script: file that contains sequence of shell commands.  
Shell scripts start with a 'shebang' line, `#!/bin/bash`, which indicates this is a bash script.  
To run a script, you must apecify the path to it:  
`$> ./basic`  
Or you can set a variable `$> mybasic=./basic` and run `$> $mybasic`.  

Script name and its arguments can be called by $N where n is a non-negative integer.  
`$> ./myscript val1 val2 ... valN`  
call:	  `$0`     `$1`	   `$2` ... `$N`  

#### Example
```
$> cat IsItAWord`
- #!/bin/bash
egrep "^$1$" /usr/share/dict/words
$> ./IsItAWord hello
- hello

```

#### Example 
A good password is not in the dictionary. Answer whether $1 is a good password.  
```
#!/bin/bash
egrep "^$1$" /usr/share/dict/words > /dev/null

```
return a status code:  
* `0` -- success
* non-zero -- failure  

We can use `$?` to check status code for last command.
