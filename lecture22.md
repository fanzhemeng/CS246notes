# Lecture 22 Visitor Design Pattern, Compilation Dependence, Casting (?)

Last Time: template classes, STL (map, vector)

## Visitor Design Pattern
Enemy has two subclasses, Turtle and Bullet.
Weapon has two subclasses, Stick and Rock.

Want the effect of strike to depend on both the type of Enemy and type of weapon.
**Double Dispatch**

```c++
Enemy *e = ...
Weapon *w = ...
e->strike( *w );    // which strike methods gets called depends on e runtime type
                    // the w type will be checked compiling time but not runtime

class Enemy {
	public:
	virtual coid strike (Weapon &w) = 0;
};

class Turtle : public Enemy {
	public:
	void strike(weapon &w) { w.useOn(*this); }
};

class Bullet : public Enemy {
	public: 
	void strike(weapon &w) { w.useOn(*this); }
};

class Weapon {
	public:
	virtual void useOn(Turtle &t) = 0;   // we create two methods since compiler knows exactly 
	virtual void useOn(Bullet &b) = 0;   // which enemy (turtle / bullet) is the parameter
};

class Stick : public Weapon {
	public:
	void useOn(turtle &t) { // using stick on turtle }
	void useOn(Bullet &b) { // using stick on bullet }
};

class Rock : public Weapon {
	public:
	void useOn(turtle &t) { // using rock on turtle }
	void useOn(Bullet &b) { // using rock on bullet }
};

Enemy *e = ... // Bullet
Weapon *w = ... // Rock
e->strike( *w );
```

#### Book Example

We want a catalogue: 

- tracks Books by author
- tracks TextBook by topic
- tracks Comic by protag

```c++
class Book {
	string title;
	string author;
	int numPages;
	static map<string, int> catalog;

	public:
	virtual updateCatalog();
};
```

Adding the catalogue code to all classes, clutters these classes.
What if source code is not available? 
We can use the visitor design pattern to add new features to a class hierachy without (1) needing source code, and (2) clutters the class hierachy.
(separation of concerns) **as long as the class hierachy accepts visitors**.

```c++
class Book {
	...
	public:
	virtual void accept(BookVisitor &b) { b.visit(*this); }
};

class TextBook : public Book {
	...
	public:
	void accept(BookVisitor &b) { b.visit(*this); }
};

class Comic : public Book {
	...
	public:
	void accept(BookVisitor &b) { b.visit(*this); }
};

class BookVisitor {
	public:
	virtual void visit(Book &b) = 0;
	virtual void visit(TextBook &tb) = 0;
	virtual void visit(Comic &c) = 0;
};
```

#### forward declaration or include header file
```c++
// a.h:
class A {...};

// b.h:
#include "a.h"
class B : public A {...};   // need to know what is inside A

// c.h:
#include "a.h"
class C { A myA; };         // need to know what is inside A

// d.h:
class A;
class D {                  // only need to know the existence of A
	A *myAp; 
	void foo();
};    

// d.cc:
#include "a.h"
void D::foo() { myAp->bar(); }

// e.h:
class A;
class E {              // forward declare A because this is a header file, 
                       // the compiler only need to know class A exists
	A foo(A x);
};
```

We prefer forward declaration when both work.

```
TextBook b(..,..,..);
TextBook::TextBook(..,..,..,..) : Book(..,..,..), topic(..) {}
TextBook c = b;   // calls the one we get for free

TextBook::TextBook(const TextBook &other) : Book(other), topic(other.topic) {}   // same as the default copy ctor we get for free

TextBook &TextBook::operator=(const TextBook &other) {
	Book::operator=(other);
	topic = other.topic;
	return *this;
};
```
