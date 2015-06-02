# Lecture 07

In C, when we do `int n=2+3.5`, the `2` convert from int to double.   
In C++, we have a conversion from `cin` to `void*`, that is, from stream to pinter.    
Pointer has a numeric value, therefore we can use the pointer in a condition.    
In C++, if condition is zero, it is false. Shell is the opposite.    
Instead of checking cin.fail(), we can just directly check cin.

### C++ conversion 
`cin` is non-zero if `cin.fail()` is not true.   
`cin` is 0 if `cin.fail()` is true.    
With if statement, we can directly check cin.    
See readInts.cc for example.   

```c++
int main() {
		int i;
		while (true) {
				cin >> i; 
				if (!cin) break;
				cout << i << endl;
		}
}
```

Note that in `cin >> i;` , `>>` is the input operator.   
In C/C++, it also is the right-bit shift operator.    
e.g. 21 = 10101 in binary, then    
		`21 >> 3` push the 21 three bits right, that is 10101 --> 10, where 
		the last three digits disappeared.

Seems like there is some ambiguity here. To determine which one should 
be used, compiler checks the types of the two arguments around `>>`.   
This is called **operator overloading**. Same operator have different 
meaning bash on "contexts".

Moreover, `cin >> i` is an expression, which has a value.
When it is evaluated, and succeed, it has the side effect: i has the 
right value.   
Expression (e.g. `cin >> i`) evaluates to `cin` (possibly mutate i).

For `cout << "Hello World" << endl;` where cout is the ostream, 
with `cout << "Hello World"` evluate to cout. Then it left with 
`cout << endl;`. And it runs until `cout;`, which finishes printing.    

This is called **cascading**.

Some questions arising: 
* What is the original value of cin? 
* What is flag?

#### Final format of readInts.cc    
```c++
int man() {
		int i;
		while (cin >> i) {
				cout << i << endl;
		}
}
```

#### Example   
Read all ints from stdin, ignore non-ints,and exit at EOF.
(readInts5.cc)

```c++
int main() {
		int i;
		while (true) { 
				if (!(cin >> i)) {
						if (cin.eof()) break;
						else {   # failed due to non-int
								cin.clear();
								cin.ignore();    # note that this two lines is the way we clear the cin fail flag and ignore it and move on
						}
				}
				else {   # read succeed 
						cout << i << endl;
				}
		}   # end while
}
```
Note that if a bad read occurs, the following reads will not happen if we do not do clear and ignore.


## String in C++

C++ has a std::string.

```C++
#include <iostream>
#include <string>
using namespace std;
int main() {
		string s;   # note the small 's' in 'string'
		cin >> s;
		cout << s << endl;
}
```
Note that here the function is space delimited, ignores leading whitespaces and terminates when encountering them.    
If want to read spaces into string variable, use `getline(cin, s);`   
`getline(cin, s)` start at current position, and read until newline (`\n`).    
**THIS IS IMPORTANT when doing assignments!!!**

#### Example   
We want to rad in age and full name and store in the corresponding variables.   
Input:
```
35\n
Nomair Naeem\n
```
The following approach is incorrect, but used by many students.    
```c++
cin >> age;
getline(cin, name);   # note here what stores in name is the empty string
```


### Printing hex number
In C, we can print hex by doing `printf("%x\n", &n);`
In C++, we print i in decimal by `cout << i << endl;`    

What if we want hex?   
C++ provides IO manipulators. 

```c++
cout << hex;
cout << i;    # print i in hex
cout << dec;
cout << i;
```
We can also do `cout << hex << i << dec;` in short using cascading.

More:   

* `showpint`
* `setprecision`
* `boolalpha`
In header:  \<iomanip\>


### File IO
Now we want IO from files. Stream abstraction can be used on other streams.   
File streams header: \<fstream\>   
When we include \<fstream\>, we get access to two variables:   

* ifstream - read from files (much like istream, cin)
* ofstream - write to files (much like ostream, cout)

On line 6 of "fileinput.cc", `type variable_name initialization` format is obeyed.   
It opens the suite.txt file

In this example and functions dealing with file IO, the file will be closed when the local variable file goes out of scope.


