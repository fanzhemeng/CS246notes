# Lecture 15 Opeator=, Operator Overloading as methods, const methods, static?

Recall:

```c++
Node &operator=(const Node &other) {
	if (this == &other) return *this;
	Node *tmp = next;
	tmp->next = other.next ? new Node(*other.next) : NULL;
	data = other.data;
	delete tmp;
	return *this;
}
```

### Copy and Swap idiom

```c++
struct Node {
	void swap(Node &copy) {
		int tdata = data;
		data = copy.data;
		copy.data = tdata;
		Node *tnext = next;
		next = copy.next;
		copy.next = tnext;
	}
	
	Node &operator=(const Node &other) {
		Node tmp = other;   // the copy constructor is called
		                    // same as Node tmp(other);
		this->swap(tmp);
		return *this;
	}
}
```

The operator= implementation above relies on correct implementationof the copy 
ctor and dtor.  
For stack variables, when the function returns, the destructor automatically
gets called.

### Rule of 3
If you need to write a custom version of 

 - copy ctor
 - dtor
 - operator=

then you usually need to write all of the three.
They are called the big 3.

## Operator Overloading As Methods

```c++
Vec v1(1,2), v2(3,4);
Vec v3 = v1 + v2;
struct Vec {
	int x, y;
	Vec operator+(const Vec &v2) {
		Vec v(x + v2.x, y + v2.y);
		return v;
	}
}

// We want to do Vec v4 = v3 * 5;

Vec Vec::operator*(const int k) {
		Vec v(x * k, y * k);
		return v;
}

// what about Vec v5 = 5 * v3; ?
// cannot implement as a method, only can be 
// a function

Vec operator*(const int k, const Vec &v) {
	return v * k;
}

// output operator as a method

struct Vec {
	ostream &operator<<(ostream &out) {
		out << x << " " << y;
	}
	// the output operator above can be worked when using parenthese in expression
	// but it is really ugly...
	// so, should not implement input and output operaor as methods
	// just make them function outside of struct def
}
```

### Arrays of objects and pointers to objects

```c++
struct Vec {
	int x, y;
	Vec(int x=0, int y=0) : x(x) y(y) {}
}
Vec vectors[3];
// stack allocated array

Vec *vectors = new Vec[size];
// heap allocated array

// the arrays above both will not compile
```

When creating an array of objects, the default ctor "must" exist.
For the struct above, the implicit default ctor goes away since a ctor is defined.
Suppose you do not want to implement a default ctor.
You can still create stack allocated array of objects, by explicitly calling a 
non-default ctor for each object.

`Vec v[3] = { Vec(1,2), Vec(3,4), Vec(5,6) };`
However, you cannot create a heap allocated array of objects if you do not have 
a default ctor. NO WAY.
Instead, create heap allocated array of pointers to objects.
Vec **myVecs= new Vec *[size];
myVecs[2] = new Vec;


### Const Methods

```c++
struct Student {
	int assns, mt, final;
	mutable int numGradecalls;
	float grade() const {
		numGradecalls++;
		return assns*0.4 + mt*0.2 + final*0.4;
	}
}

const Student bobby(60, 70, 80);
// cannot change bobbys fields
// can we call bobby.grade() ?
// grade() does not change fields
// it should be ok if grade declares it does not change fields
// that is, it is a const methods
```

Note that const objects can only call const methods.
If you want to change field vlaues even if object is a const, use mutable keyword.
Methods that only mutate mutable fields are sill considered const methods.
For strcut contains const pointers, I can not change the pointer, but we can 
change the value the pinter is pointing to.

