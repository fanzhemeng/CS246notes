# Lecture 09 Short topics, Review, References

## Recap: Declaration before use

## Pointers
```c++
int n=5;
int *p = &n;
cout << p << endl;  // print address in hex
cout << *p << endl;  // print the value
int **pp = &p;
cout << pp << endl;
cout << *pp << endl;
cout << **pp << endl;


__________
|________|
|___5____|  n: 10,000
|________|
|_10000__|  p: 48,000
|________|
|_48000__|  pp: 60,000
```

## Arrays
Array Represents collection of values

```c++
int a[] = {10,12,13,14};


__________
|________|
|__10____|
|__12____|
|__13____|
|__14____|
|________|
```

Arrays are not pinters.  
The name of an array is short hand for the address of the first element of the array.  
That is, `a = &a[0]`;  
Also, `*a = a[0]`,  
`*(a+1) = a[1]`.

## Structures
In C, do not forget the semicolon at the end of the struct def.  
In C++, after writing word "struct" in the struct def, we do not need to write 
"struct" after when defining variables.

If we do not use pointer when defining a linked list, the def keeps repeating 
itself revursively, and will not stop.

## Constants
A const def must be initialized.

```c++
int n = 5;
const int *p = &n;
// We can do `p=&m` and `n=10`, but not `*p=10`.
int * const pp = &n;
// Here, we can do `*pp=10`, but not `pp=&m`.
const int * const ppp = &n;
// we can do neither.
```

"const" applies to the thing on the left unless there is nothing on 
the left, then right.

## Parameter Passing
"pass by value" VS. "pass by pointer"

In C,
```
int x;
scanf("%d", &x);
```

In C++, `cin >> x;`  
It is in fact a function call : `operator>>(cin, x);`  
Note the "x" in the functional call.  
This is "pass by reference"

## Reference
```c++
int y=10;
int &z=y;       // z is a reference to y
z=100;
int *p = &z;    // &z gives the address of y

__________
|________|
|_100____| y
|________|
|________| z
|________|
|________| p
(should be some arrows going on...)
```

A reference acts like a constant pointer with automatic dereferencing.  
We cannot know the address of z.  
z is now another name for y. Or we can say, z is an alias for y.  
z has no identity of its own.

Restrictions:

1. References must be initialized
2. References must be initialized to something that has an address, this 
   something can be called "storage location" or "lvalue". 
	 so, we cannot do `int &n=5;`, or `int &m=y+y;`
3. not able to create a pointer to a reference
4. not able to create a reference to a reference
5. not able to create an array of references

```c++
void inc(int &n) {
	n++;
}

int main() {
	int x=5;
	inc(x);         // int &n = x;
	cout << endl;   // prints 6
}
```

For `cin >> x;` it is in fact the function call: operator>>(cin, x);  
The function is internally implemented by the following function signitrue:  
`istream &operator>>(istream &in, int &n);`  
Note that here: 

- `istream &` is the return type.
- `operator>>` is the function name.
- `istream &in` and `int &n` are parameters.
- The return type "istream &" allows us to cascading, like `cin >> x >> y;`.

#### Example
```c++
struct ReallyBig {...};
void foo(ReallyBig rb) {
	...
}

ReallyBig lib;
foo(lib);       // a copy of lib is made
```
In the code above, we use "pass by value" approach, the advantage is that changes 
to rb are invisible outside `foo(rb)`, that is, changes are local scope in foo.  
So pass by value is still needed in some cases.


In C, we could have sent a pointer to lib, like 
```
void goo(ReallyBig *rb)
```
What happens with this?

- a copy of the data structure is not made
- changes are visible outside foo


```
void goo2(const ReallyBig *rb)
```
What about now?

- no copy
- no changes allowed
- cannot use rb to change lib


In C++ only, we can do  
```
void hoo(ReallyBig &rb)
```
Now? 

- a copy is not made
- changes are visible outside
- benefit over pointers: automatic dereferencing
- use pass reference to const (optional, differs regarding goals) for anything greater than an int

#### How to use references
```c++
int f(int &n);
int g(const int &n);
f(5);           // will not work, must initialize with an lvalue
g(5);           // will work
int y=1;
f(y+y);        // will not work
g(y+y);        // will work
```
