# Lecture 08  string, short topics, and review topics

Anything that we can do with an istream,for example cin, we 
can do with ifstream (same for ostream and ofstream)    
C++ provides a string stream abstraction.    
header: `<sstream>` provides two types: 

* `istringstream`: treat a string as a source of input
* `ostringstream`: write output to a string

#### Example:   
	 
```c++
int l = ...;
int h = ...;
ostringstream ss;
// we can treat 'ss' the same way as we treat cout
ss << "Enter a number between" << l << "and";
ss << h << endl;
cout << ss.str();   // create a string altogether before sending it to cout, or any other ostream, such as ofstream.
```

Possible use of `istringstream`: convert a string into an int. See getNum.cc for example.   
Note that on line 12, we initialize ss with the value of s.
We do not have to do clear and ignore with istringstream.
Note that ss is a local variable in the while loop, so if read fails, a flag does go up, but we just throw the current ss away, and create a new one.     
However, the function will not work when we give no input and Ctrl+D, since the while loop will not stop. Because if a fail flag of cin goes up, and we do not clear it, then cin is not going to read anything again.    
Compare readIntsS.cc to readIntsSS.cc for details.


## `std::string` in C++
In C, strings are char* that are null terminated. This Causes problems:   

- Requires explicit memory management
- very easy to overwrite the null terminator ('\0')

C++ has the	`std::string` type. We do not have to worry about the null terminator when dealing with std::string. It has the advantages of:   

- automatic growth and shrink based on length
- much safer

Note that string type in C is different from that in C++.   
`string s = "Hello"; // variable type on the left is std::string, and on the right is C-style string. "H | e | l | l | o | \0"`   
We can do this because C++ allows an implicit conversion from C-style string to std::string.
Also, string length of a std::string does not include the null terminator.   


## String Operation
### Equality 
In C, `s1 == s2` does not work if s1 and s2 are char*, instead we can use `strcmp(char*, char*)`.   
In C++, we can apply `==`, `!=`, `<=`, `>=`, `<` , `>` directly to s1 and s2.

### String Length
In C, we use `strlen(char*)` to get length.   
In C++, we use `str.length()`.

### String Concatenation
In C, if s1 and s2 are char*, we do `strcat(s1, s2)` to append s2 to the end of s1. Note that s1 is mutated. We have to make sure s1 has enough space to allow that.   
In C++, we use `s1+s2` to do it, which will create a new value of appending s1 and s2 together, and we can store it in a new variable, `s3 = s1 + s2`.

#### Example
```c++
void printFile(string name) {
	ifstream file(name); // we can only initialize a ifstream variable with C-style string, but not C++ std::string
	string s;
	while (file >> s) {
		cout << s << endl;
	}
}
```
The function above will not compile because the assigned value to the variable `file` is not the type it requires.   
To fix the problem, we can create a C-style string from std::string.   
To get a C-style string from std::string, use:
`name.c_str(); // name is a std::string`

Note that conversion in initialization of a std::string only goes from C to C++.   
See file readSuites.cc for the correct function and details.


## Default Arguments

#### Example
```c++
void printFile(string name="suite.txt", int num) {
	...
}    // this will not work, we can not make a parameter with default value followed by a parameter without a default value.
```
We cannot follow a parameter with a default value with one that does not have a default value. But we can do the reverse.

#### Example

```c++
void test(int num=0; string str="Waterloo") {
	...
}

test(); // use the default arguments
test(5); // change the num to 5
test(10, "Montreal");  // change both arguments
test(, "Toronto"); // will not work!
```

## Function Overloading

In C, 
```
int negInt(int a) {
	return -a;
}

bool negBool(bool b) {
	return !b;
}
```

In C++, 
```
int neg(int a) {
	return -a;
}

bool neg(bool b) {
	return !b;
}
```

This is called function overloading, which is the ability to use the same name for 
two or more functions as long as the number and/or the types of parameters are different. 
But it is not sufficient to just differ on the return types.

```
void twoParams(int, string) 
void twoParams(string, int)   // allowed
int twoParams(int, string)   // not allowed

void test(int num=0, string str="Waterloo")
void test();
void test(int);
void test(int, string); // all above are not allowed
void test(string);   // can do this?
```

When we do `cin >> x;`, what the compiler really does is: `operator>>(cin, x);`.     
`operator>>(21, 3);` for `21 >> 3;`.


## Declaration before Use
Forward declaration, same as in C.

