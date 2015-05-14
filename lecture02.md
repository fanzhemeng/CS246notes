# Lecture 02

## Globbing, pipes

### Linux FIle System
	"Ordinary" File: mp3, programs
	Directories: files that can contain other files

	e.g.			/ (root directory)
			/	 /		\		\
		bin		etc		home	usr
		/		/		\		/	\
	bash	shell	nanaeem	  bin	include
					/	\
				cs241	cs246
						/	\
					1151	1155		(discussed in last lecture)

### PATH: specify the location of any file in the file system
	
	* Absolute Path: a path that starts at the root directory
		`/bin` : the bin dir inside /
		`/usr/bin` : the bin dir under usr under /
	
	* Current Directory: the directory you are currently sitting in
		`$> pwd` shows current working directory
	
	* Relative Path: specify a file location relative to the current directory
		e.g. Suppose pwd is `/home/nanaeem/cs246`, then the absolute path 
			`/home/nanaeem/cs246/1155/a0/a0.pdf` is the same as the relative 
			path `1155/a0/a0.pdf`
		`$> cd path` change directory, where path is an argument for cd

	* Special Directory:
		1. `.` (DOT) represents the current directory
		2. `..` (DOT DOT) refers to the parent directory of the current dir
				e.g. current dir is /home/nanaeem/cs246/1151
					`$> cd /home/naneem/cs246` 
					`$> cd ..`
					`$> cd ../1155`
					Grandparent? `$> cd ../..`
		3. `~` (tilde) is the home directory
				e.g. `$> cd ~/cs241`
					 `$> cd ~`
					 `$> cd ` also goes to the home directory
		4. `~userid` changes into userids home directory
				e.g. `$> cd ~nanaeem`
		5. `$> cd -` changes to the last directory you were in
### LISTING COMMAND
	* To list files: 
		`$> ls` lists all files in the current directory except for hidden files (which starts with a .)
		`$> ls -a` lists all files including the hidden files
	* Wildcard Matching (Substitution):
		- Want to find all files that end with .txt: 
			`$> ls *.txt` where * is the globbing pattern, matching anything that ends with .txt
		Q: What happens when a globbing pattern is used?
		A: The shell notices the globbing pattern, performs the wildcard substitution, and replaces the 
		   glob with the results.

	tab does the auto-completion.
	`$> echo` do not know what echo does yet.....
	single or double quote for *.txt, i.e. to suppress wildcard matching

	* Examples:
		`$> cat sample.txt`	// displays the content of sample.txt
		`$> cat`	// killed by Ctrl+C
		`$> cat > output.txt` // EOF (Ctrl+D) 
		`$> cat >> text.txt` // appends to the end of  text.txt
		`$> cat sample.txt` argument to cat
		`$> cat < sample.txt > bla.txt`
		* Note: `<` is the input redirecton 
