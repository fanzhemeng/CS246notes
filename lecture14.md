# Lecture 14: explicit operators

```c++
struct Node {
	int data;
	Node *next;
	Node(const Node &other) :    // deep copy constructor
		data(other.data)
		next(other.next ? new Node(*other.next) : NULL){}
	explicit Node (int data) : data(data)
                             next(NULL){}
};

void foo(Node n) {
}

Node n(4);     // valid
Node n = 4;    // valid
foo(4);        // valid
```

One-parameter constructor create implicit conversions
e.g. `string str = "Hello";   // from C-style to std::string`

If want to disallow the conversion, we use the keyword `explicit`
like in the example above.

## Destructors (dtor for short)
Languages which do not have automatic memory management requires 
you to manually manage it. objects are destroyed by calling the
destructor.

 - stack allocated objects are destroyed when they go out of scope,
   destructors called automatically
 - heap allocated objects are destroyed when delete is called, which 
   calls the dtor
 - we get one dtor for free, the default dtor, which automatically 
   calls the dtors of fields that have them
 - you can only have one dtor for one class
 - e.g. `~Node()`, the name of the class with a ~ prefix

```c++
Node *np = new Node(1, new Node(2, new Node(3, NULL)));

// If no deletes, only np is reclaimed.
// leaks memory

np --> |_1_|_| --> |_2_|_| --> |_3_|_/_|
// (linked list above)


struct Node {
	...
	~Node() {
		delete next;     // `delete` calls the dtor for the object `next`
	}
};
```

It is always safe to delete a NULL pointer. However, it is not safe to
delete a pointer twice.

```c++
Node *np = new Node(1, new Node(2, NULL));
Node *second = np->next;
delete np;
delete second;              // double free error, should not do this
```

After deleting, you get a dangling pointer, which you cannot depend on.

### Separate Compilation
For a class, we put the class def, including method headers in .h files, 
and put hte implementation of the methods in the .cc files

If you implement a class method outside of the class def, should specify
the scope of the method. use ::, the scope resolution operator.
<classname>::<methodname>   there is a few operators cannot be overloaded

It is good style to put every type into different .h and .cc files

## Assignment operator

```c++
Student bobby(60, 70, 80);  // calling ctor
Student billy = bobby;      // calling copy ctor
Student jane;               // 0-parameter ctor
jane = bobby;               // assignment operator
```

what the assignment operator really looks like ? 
`jane.operator=(bobby);`

```c++
struct Node {
	Node &operator=(const Node &other) {
		data = other.data;
		next = other.next;
		return *this;        // `*this` dereferences this pointer
	}
};

n1 = n2 = n3;
n1.operator=(n2.operator=(n3));

We want deep assignment, which is implemented below.

```c++
Node &Node::operator=(const Node &other) {
	if (this == &other) return *this;  // checks self assignment
	data = other.data;
	delete next;                       // without this, we would leak memory
	next = other.next ? new Node(*other.next) : NULL;
	return *this;                      // cascading
}

Node *np = new Node(1, new Node(3, NULL));
*np = *np;   // self assignment
```

An exception. when run out of heap memory
