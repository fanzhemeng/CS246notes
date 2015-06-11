# Lecture 10: Dynamic Memory, More Review, Operator Overloading, Preprocessor

In C, we use `malloc` and `free` to access dynamic memory.  
```c
int *p = malloc(sizeof(int)*10);
free(p);
```

In C++ while we do have `malloc` and `free`, we do not use them.  
We use `new` and `delete` in C++ because of the following benefits of them:

- type aware
- less prone to errors

#### Example

```c++
struct Node {
	int data;
	Node *next;
};

int main() {
	Node n;                 // on the stack
	Node *np = new Node;    // "new" knows how much space is needed for a Node
	delete np;              // once done, call delete
}
```

## Allocating dynamic Arrays

```c++
int *arr = new int[size];
delete [] arr;
```

#### Review: Stack vs. Heap Allocation

```
Memory (RAM)
_______________
|_____________| code
|_____________|
|_____________|
|_____________|___
|_____________| heap
|_____________|
|_data:10_____| 0x12345
|_next:NULL___|
|_____________|
|_____________|___
|_____________| stack
|_____________|
|_0x12345_____| np
|_____________|
|_data:5______| n
|_next:NULL___|
|_____________|
```

## Operator Overloading

#### Existing Example
```c++
cin >> x;
21 >> 3;

3 + 5;
s1 + s2;
```

**IDEA**: we can give meaning to c++ operators for our own types.

### Vector Example
```c++
struct Vec {
	int x;
	int y;
};
```

#### Vector Addition
In C, a function for adding two vectors looks like below.

```c
Vec add(Vec v1, Vec v2) {...}
add(v1, v2);
```

In C++, we would instead write v1+v2 by overloading the + operator.

```c++
Vec operator+(const Vec &v1,const Vec &v2){
	Vec v;
	v.x = v1.x + v2.x;
	v.y = v1.y + v2.y;
	return v;
}

Vec a = {1, 2};
Vec b = {3, 4};
Vec c = a + b;        // c is {4, 6}
Vec d = c + a + b;    // not able to call this unless in operator 
                      // def we have "const" before parameters
```

#### Scaler multiplication of vectors

```c++
Vec operator*(const int k, const Vec &v1) {
	Vec v;
	v.x = k * v1.x;
	v.y = k * v1.y;
	retirm v;
}

Vec a = {1, 2};
Vec b = 5 * x;

Vec operator*(const Vec &v, const int k) {
	return k * v;
}
```

### Overloading << & >>

#### Grade Example

```c++
struct Grade {
	string name;
	int grade;
};

Grade g = {};
cout << g;     // will not work

ostream &operator<<(ostream &out, const Grade &g) {
	out << g.name;
	out << " has the grade: "
	out << g.grade << "%" << endl;
	return out;
}

Grade g;
cin >> g;     // will not work
istream &operator>>(istream &in, Grade &g) {
	in >> g.name;
	in >> g.grade;
	if (g.grade > 100) g.grade=100;
	if (g.grade < 0) g.grade=0;
	return in;
}
```

## Preprocessor

```
sample.cc --> preprocessor --> compiler --> ...  
             |--------- g++ ----------|  
```
Whenever you write something with a "#", you are using a preprocessor directive.

```
#include <iostream>  // include a c++ standard header
#include "file.h"    // include this particular header which is in current dir
```
"#include" does a copy and paste.  
`$> g++ -E filename` only runs the preprocessors of file.

```
g++ -E -P filename
```

- only runs the preprocessor
- omits the line numbers

### Preprocessor Directive #define
`#define VAR VALUE`  
For example, `#define MAX 10`  
`#define` does Search and Replace

`#define` predates the `const` keywords, but we should still use `const`.
