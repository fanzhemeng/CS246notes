# Lecture 06

For the assignments (2-4):    
* due date 1 is for all test cases.  
* due date 2 is for code submissions.

## Testing 
### Black Box Testing
- no knowledge of how the program is coded
- Functionality  
	e.g. for stack, check the 'first in, last out' rule.  
- Valid vs.Invalid input
	* invalid input: In CS 246 we do not provide invalid input, so we should not worry about it, for now. 
	* valid input
		* +/- values
		* boundary cases: extremes of a single case
		* edge cases: extremes of two ranges

Note that you should make your test cases small and specific.

### Functional testing (whitebox testing)
* Test converage 
	* execute functions
	* take all possible paths

### Regression Testing
Check if your revision has changed anything that was correct.


## C+++++++++++++++
Take the OOP concepts into the C language.   
80s B.S. (could not catch the name XD) created C with Classes.   
versions: C++03, C++11, C++14   
We will focus on C++03 in this course.   

Most C programs are valid C++ programs, unless the keywords (such as `new`, `print`, etc.) 
were mistakenly used as parameters.

#### Example
Write HelloWorld in C++  
``` c++
#include <iostream>
using namespace std;
int main() {    # return must be int, which is a status code
		cout << "Hello World" << endl;
		return 0;  # optinal, can be omitted 
}
```

Note that `stdio.h` (printf) are still available in C++, but not encouraged, 
and it will be detected by Marmoset. So **FORBIDDEN**.

### C++ approach  
* include iostream
	* variable  `std::cout`
	  e.g. `std::cout << "Nomair" << std::endl`  
		* std is a namespace, in which there are cout and endl
		* endl is the newline character regarding your systems

* compiling
To compile a C++ course code file,  
``` bash
$> g++ prog.cc -o prog		# -o create a executable file called prog
$> ./prog
```

Note that `$> g++ prog.cc` automatically create an executable file, called 
a.out, in the current dir.

When you include `iostream`, you import 3 variables, 
* cout - send to stdout
* cin - read from stdin
* cerr - send to stderr

### I/O operator
* `<<` - output operator (put to operator)
	e.g. `<< x;` send value of x to standout, and
	`cerr << 'ERROR';`
* `>>` - input operator (get from operator)
	`cin >> x;` get from cin and put to x

#### Example

``` c++
int main() {
		int x=0;
		cout >> x;		# note here the '>>' should be '<<'
}
```

The code above will generate a warning on Mac, but failed on Linux.   
Therefore, do not depend on the outcomes when running code locally.

#### Example (C++/intro/plus.cc)
Sum of two number.
``` c++
int main() {
		int x,y;
		cin >> x >> y;
		cout << x+y << endl;
}
```

`cin` ignores whitespaces, but reads in any other values.
If a read fails, the behaviour of the function is undefined, and we cannot
rely on the output returned in this case.   
Ctrl+D means end of file (EOF). (same as in CS136)

Now we want to detect if read fails or not.  
If a read fails  
* for wahtever reason below, 
	* read something unexpected, or
	* encountered EOF (Ctrl+D)   
	then `cin.fail()` is true.
* due to EOF, then `cin.eof()` is true

#### Example  (C++/io/readInts.cc)
Read all ints from stdin and echo to stdout, and stop if a non-int is encountered or EOF is hit.

``` c++ 
int main() {
		int i;
		while (true) {
				cin >> i;
				if (cin.fail()) break;
				cout << i;
		}
}
```

