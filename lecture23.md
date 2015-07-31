# Lecture 23  Partial a mixed assignment, casting, exception

Recap:
When implementing the copy ctor / operator= in the present of inheritance, the operator on the superclass fuist, then the subclasses.


```c++
TextBool b1("CS246", "Nomair", 200, "C++");
TextBook b2("CS136", "Dave", 100, "C");
Book *pb1 = &b1;
Book *pb2 = &b2;
*pb1 = *pb2;
```

Object assignment through pointers.
print b1 after the assignment, print CS136, Dave, 100, C++

Book::operaotor= executed
partial assignment
compiler checks the declared type of pb1 (Book *).
since operator= is not virtual, Book::operator= executes.

Let us make operator= virtual. 

```c++
virtual Book &Book::operator=(const Book &) {...}
TextBook &TextBook::operator=(cosnt Book &b) { // parameter must be Book to be valid
// how do I access RHS topic 
}
```

mixed assignment problem
```c++
TextBook b(..,..,..);
Comic c(..,..,..);
b = c;
b.operator=(c);
```

we want make operator= virtual. 
Let us make Book::operator= protected.
This would work, but it disallows actual Book objects to be assigned.

**Advice**: make base classes abstract, and implement a protected operator=.

Have a AbstractBook, has fields, title, author, numPages, has method, #operator=(..).
Subclasses: NormalBook, has no field, and method, +operator=(NormalBook &): NormalBook &
And TextBook and Comic subclass remain unchanged as before.

```c++
TextBook b1(..), b2(..);
AbstractBook *pb1 = &b1;
AbstractBook *pb2 = &b2;
*pb1 = *pb2;   // will not compile, because AbstractBook::operator= is protected
```


## Casting

In C and C++,
Node n;
int *p = (int *)&n;   // C-style tend to get lost in the code

Too powerful.

### staic_cast

for casts that make "sense".
```c++
int m = 9;
int n = 2;
cout << w / n << endl;  // prints 4 because integer division
cout << static_cast<double>(m) / n << endl;   // prints 4.5

Book *pb = ...; // suppose pb points to a TextBook
TextBook *t = static_cat<TextBook *>(pb);
```

This is unchecked cast, tells compiler: trust me, i know what i am doing.
Behaviour is undefined if cast is incorrect.

static_cast can only be used if there is an Is-a relationship. called downcast


### reinterpret_cast

No restrictions. 

```c++
Vec v;
Student *s = reinterpret_cast<Student *>(&v);
```

completly copiler dependent.
relies on how objects are represented in memory.


### const_cast

- add/remove const
```c++
void libraryfunc(int *g) {...}
function does promise not to change g
void foo(const int *p) { libraryfun(p); }  // cannot do this
librayfunc(const_cast<int *>(p));   // remove the const type of p
```

Behaviour undefined if libraryfunc() changes g.


### Dynamic_cast
```c++
vector<Book *> collection;
for (...) {
	TextBook *t = dynamic_cast<TextBook *>(collection[i]);
	// tentatively check
	if (t) cout << t->getTopic() << endl;  // check
	else cout << "Not a TextBook" << endl;
}
```

if the cast succeeds, ie. collection[i] is a TextBook then t points to it.
Otherwise t is set to NULL.

Dynamic_cast only works if the class hierachy has at least one virtual method.




## RTTI runtime type information

```c++
string whatisit(Book *b) {
	if (dynamic_cast<TextBook *>)(b)) {  // b points to a tb
		cout << "TextBook" << endl;
	}
	else if (dynamic_cast<Comic *>(b)) {
		cout << "Comic" << endl;
	} else cout << "Book" << endl;
}
```

high coupling, may not do this.

