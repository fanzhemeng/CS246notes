# Leture 12 Include Guards, C++ Classes

see separate/example4 directory

Summary for 'include'

- always put include guards in header files
- never compile .h file
- never include .cc files
- never put `using namespace std;` in header files
- use `std::iostream`, `std::string`, `std::cout` etc instead

## C++ Classes

Big innovation of OOP: **you can put functions inside a struct.**

### Example
```c++
struct Student {
	int assns, mt, final;
	float grade() {
		return 0.4 * assns + 0.2 * mt + 0.4 + final;
	}
};

Student billy = {70, 60, 75};
billy.grade();
```
(Billy shenmegui)

A class is a struct that contains functions.   
All c++ structs can contain functions, which implies they are all classes.

An instance of a class is called an **object**. A function that is inside 
a struct/class is called a member function, or a method.
  - Student is a class, billy is an object. `grade()` is a member function.

Note that a method can have no explicit parameters. When we call a method
on objects, then only inside the method, can you have access to the object 
fields.
 - in	`billy.grade()`, grade has access to billys assns, mt, final marks.

#### Key diff between functions and methods

A method has a hidden parameter called `this`, which is a pointer pointing 
to the object on which the method was called.  
	 - `this == &billy`, and `*this == billy`

#### Rewrite the class `Student`

```c++
struct Student {
	int assns, mt, final;
	float grade() {
		return 0.4 * this->assns + 
		       0.2 * this->mt +
					 0.4 * this->final;
	}
};
```

## Initializing objects

```c++
student billy = {70, 60, 75};    // c-style initialization
```
This is a restrictive way of initialization.

In c++, we can initialize / construct objects by calling constructors.

```c++
struct Student {
	int assns, mt, final;
	float grade() {...};
	Student(int assns, int mt, int dinal) {  // function has same name as the class, no return type
		this->assns = assns;
		this->mt = mt;
		this->final = final;
	}
};

Student billy(60, 70, 80);                 // syntax similar to istringstream ss(s);
Student billy2 = Student(60, 70, 80);       // they are equivalent
                                           // and they are on the stack

Student *pBilly = new Student(60, 70, 80);  // this is on the heap
delete pBilly;
```

We can give the parameters some default values.

```c++
struct Student {
	int assns, mt, final;
	float grade() {...};
	Student(int assns=0, int mt=0, int dinal=0) {
		this->assns = assns;
		this->mt = mt;
		this->final = final;
	}
};

Student bobby(60, 70);      // bobbys final = 0
Student Johnny;             // zero parameter, note that there should be no parentheses
Student Johnny();           // will not work, looks like a function declaration
```

### New Knowledge
Every class comes with a built-in zero parameter constructor, which: 

- calls the zero parameter constructors on all fields that have constructors 
	(i.e. those fields of objects, not primitives such as ints, pointers, etc.)
- it is called the ** default constrcutor **

#### Example

```c++
strcut A {
	int x;
	Student y;
	Student *z;
};

A myA;            // here we call the free 0-parameter constructor for A:
                  // x is a primitive, no ctor, not iniliaized
									// y is an object, its default (0-param) ctor will be called
									// z is a pointer, no ctors, not initialized
```


```c++
struct Vec {
	int x, y;
};

Vec v;          // 0-param ctor does nothing as both fields are basic / primitive types
```

Note that **the default ctor goes away as soon as you write any ctor **.

```c++
struct Vec {
	int x, y;
	Vec(int x, int y) {
		this->x = x;
		this->y = y;
	}
};

Vec v;           // will not compile, because the 0-param ctor goes away
                 // but will compile if ctor has default args
```

If you write a ctor, you cannot use c-style initialization anymore.
  - `Vec v = {1,2};` will not work under the structure def above.

### Constants in struct

```c++
struct MyStruct {
	const int myConst = 5;         // must initialize consts and refs
	inr &myRef = m;                // m is a global var
};

// we must initialize consts and refs.  
// however, we cannot initialize fields as part of declaration.

MyStruct a;
MyStruct b;
a.myConst == b.myConst;

Struct Student {
	const int id;
};
```
initializing in the ctor body is too late, c++ will not allow that.

Three Steps that occur when an object is created:

1. space is allocated (stack or heap)
2. fields initialization: default ctors for fields 
   that hae ctors are called
3. ctor body runs

We want to initialize const and refs between step 2 and 3.  
How? Next lecture.
