# Lecture 16 Static Keyword, Design Pattern: Singleton, Encapsulation: public/private, class

### Static Keyword
Members of a class (fields and functions) can be declared static.  
static fields are associated with the class, not objects of the class.

```c++
struct Student {
	static int numObjects;  // this is a declaration, where do you initialize numObjects? 	
	Student(): ... {
		++numObjects;  
	}
};
```

A static field must be defined outside (external) to the class declaration.  
But since we have .h and .cc files, it is doable.  
In .cc file,  

```c++
int Student::numObjects = 0;  // :: is the scope resolution operator

int main() {
	Student billy;
	Student bobby(billy);
	cout << Student::numObjects << endl;  // print 2
}
```

### Static Member Functions
We can also make a static function in the class. A static function do 
not need an object to be called.   
Thus we are not given the  access to the "this" pointer. 

- Does not need an object to be called.
- There is no "this" pointer.

```c++
struct Student {
	...
	static int printNumObjects() {
		return numObjects;
	}
};
```

call this:  
`Student::printnumObjects();`

Static memebr functions can only call other static functions, and 
can only access static fields.


## Design Pattern
A suggestion of a way to get functionality of your implementation.  
If *this* is your problem, then *this* is the solution to your problem.

### Singleton Pattern
We have a class C, we want to ensure that out program only create 
one object of this class.

#### Wallet Example

* wallet class 
	* singleton
* expense class 
	* will need access to the wallet

Summary 

- static pointer to a wallet
- static function to provide access to the wallet

o_O wallet.cc and wallet.h: 

declaration .h file:

```c++
struct Wallet {
	static Wallet *instance;
	static Wallet *getInstance();
	Wallet();
	int money;
	void addMoney(int amt);
};
```

implementation:

```c++
Wallet *Wallet::instance = NULL;
Wallet *Wallet:getInstance() {
	if (!instance) {
		instance = new Wallet;
	}
	return Instance;
}

...
```

### atexit function in \<cstdio\>
takes one parameter, which is a pointer to a function.
This function must not take any parameters, and must have void type returned.
We are trying to find a way to execute a function when the prog is terminating, 
to solve the memory leak issue.

example: se/singleton/2-without-class  
cleanup() is implemented using atexit function.

For A3Q4, we should do multiple calls to atexit stack up, and last atexit function 
registered runs first.



## Encapsulation
The singleton pattern only works if no expenses directly calls the Wallet ctor.

Key idea of encapsulation: control the way objects are used

- Create a capsulation around your object.
  - Client can interact through the exposed interface.
```c++
struct Vec {
	Vec(int x, int y) : x(x), y(y) {}
	private:  // label
		int x, y;
	public: 
		Vec &operator+(...);
}
```

Private implies code that is outside this class cannot access this "private" members.

```c++
int main() {
	Vec v(1,2);
	cout << v.x << " " << v.y << endl;  // not work
	v.bar();  // not work
}
```

**ADVICE**: make all fields private.

Note that default visibility in struct is public.
Why not making that private? It will violate the structs in C.

C++ introduces the class keyword. The only difference between a struct and a class is 
that the default visibility of a class is private, while that of a struct is public.

```c++
class Vec {
	int x, y;
	void bar();
	public: 
		Vec(...);
		Vec operator+(...);
};
```
example: singleton/3-using-class

