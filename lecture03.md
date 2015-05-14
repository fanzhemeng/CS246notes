# Lecture 03

difference between `$> wc sample.txt` and `$> wc < sample.txt`:

	* the name of the file `sample.txt` will not be showed if using input redirection

genenral form involving redirection: `command args < input > output 2> error`

	- In Unix/Linux systems,

	```

																														    |--> standard output stream (stdout)
		  stanard input stream (stdin) --> prog / process command --| 
																														    |--> standard error (stderr)

	```

		* stdin by default: keyboard
		* standout/standerr by default: screen
		* `<` input redirection changes stdin source 
		* `>` output redirection changes stdout source

	- stderr is separate from stdout because: 
			1. do not want to clutles prog output with error msgs
			2. stdout is buffered, while stderr is not
			* However, we can use `command 2> error.log` // second out stream which is the error stream

			  Moreover, `$> 2>&1` //save error msgs to the same file of the output file

eg1. How mang words in the first 20 lines of sample.txt?

		`$> head -20 sample.txt > temp`

		`$> wc -w <temp`

		commands above will create a temp file , but if using pipe:

	* Pipe: connect the output of one process to the input of the next

			sample.txt -->	head  --> stdout (20 lines) --> wc --> answer

		in this example, `$> head -20 sample.txt | wc -w`

		or, `$> cat sample.txt | head -20 | wc -w`

eg2. Suppose files words1.txt and words2.txt contains list of words one per line.

	 Print a duplicate free list of all words.

	`$> cat words*.txt | sort | uniq`
	
	* These are called linux command pipelines

## Pattern Matching Within Files
	* egrep

		- used to use `grep`: global regular expression print

		- Now use `egrep`: extended grep

		- `egrep` is equivalent to `grep -E`

			`$> egrep pattern files` ouputs lines that contain a string that match the pattern

		- e.g. Print all lines from index.shtml that contain cs246.
			
```
			`$> egrep cs246 index.shtml`
			`$> egrep cs246 index.shtml | wc -l`
			`$> egrep 'cs246|CS246' index.shtml | wc -l`
			- Note that without the quotes, the command above will not work
			`$> egrep "(cs|CS)246" index.shtml | wc -l`
			`$> egrep "(c|C)(s|S)246" index.shtml | wc -l`
			`$> egrep "[cC][sS]246" index.shtml | wc -l`
			- Note that `[abcd]` is the short hand for `a|b|c|d`
			- `[^abcd]` choose any one symbol but not the ones in the set.
```

	* `?` represent 0 or 1 of the previous sub-pattern
		- e.g. `"cs ?246"` and `"(cs )?246"`

	* `*` means 0 or more of the proceeding sub-pattern
		- e.g. `"(cs)*246"` represents 246, cs246, cscs246, ...

	* `.` (DOT) represents any one symbol
		- e.g. `"cs.*246"`

		`"^cs246"` represents lines starting with cs246

		`"cs246$"` represents lines ending with cs246

		`"^cs246.*cs246$"`

	* `\` is the escape character

		`"\."` represents a line that contains an actual dot

	* `+` means 1 or more of the proceeding sub-pattern

		e.g. `".+"`, `".(.)*"`

