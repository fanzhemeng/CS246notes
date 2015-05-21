# Lecture 04

## Regular expression special characteristics
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
Commands: `$> ls -l `  // long listing

Output: `-rwxrw-r-- 1 nanaeem staff 28 23 Feb 2015 08:24:38 fname.txt`

* Type (the first column)
	* `-` -- ordinary files
	* `d` -- directories
	* `l` -- symbolic links

* Bits
Three kinds -- user bits, froup bits, and other bits
	* user bits: owner permission
	* group bits: other users in the group 
	* other bits: all other users

Three options:
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
`$> chmod mode file `

* mode
	* ownership 
	`u` for user, `g` for group, `o` for others, `a` for all
	* operator
	`+` for add, `-` for reduce, `=` for set to
	* permission
	`r`, `w`, `x`

Note that only owner can change the permissions. 
(but copying changes ownersip, should read more about copy)


