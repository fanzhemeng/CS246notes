# Lecture 17 Access, Mutation, friends, system modelling, Relationships

Last time: 

- Keyword, static, 
- Singleton Pattern
- Encapsulation  
  Keywords: public, private, class

### Recap:
```c++
class Vec {
	int x, y;
	public:
	Vec(int x, int y) : ... {}
};
```

x and y cannot be accessed by functions outside of the class definition.

**Advice**: make all fields private.

 - flexibility to change the implementation
 - maintain class invariant (sth that is always true in class)  
   client code will not be able to change the rule set by you

We can still provide accessors / mutators (getters / setters)

```c++
class Vec {
	int x, y;
	public:
	int getX() const {return x;}
	int getY() const {return y;}
	void setX(int x) {this->x = x;}
	void setY(int Y) {this->y = y;}
};
```

Getters should always be const methods.

### Frineds

Suppose:

- a class has private fields
- no accessors (getters)
- we want to implement the output operator ( << )  
  - operator<< must a function

Solution: use a frinend keyword

```c++
class Vec {
	int x, y;
	public:
	friend ostream &operator<<(ostream &, const Vec &);
};
```

Note that operator<< is still a function, not a method.

```c++
ostream &operator<<(ostream &out, const Vec &v) {
	out << v.x << " " << v.y;
	return out;
}
```

Friendship break encapsulation. Have as few friends as possible.

## System Modelling (UML)
- identify the main abstractions / entities / classes
- the relationsip between entities

UML: unified modelling languages

A class is represented by a box

```
__________________________
|__________Vec___________|    // name 
|  - x: Integer          |    // fields
|  - y: Integer          |    //  - for private
|------------------------|
|  + getX: Integer       |    //  + for public
|  + getY: Integer       |
|  + setX(integer): void |
|________________________|
```


### Relationship

### Composition "owns a"
```c++
class Vec {
	int x, y, z;
	public:
	Vec(int x, int y, int z) : x(x), y(y), z(z) {}
};

Class Plane {
	Vec v1, v2;
};

Plane p;      // does not work, no default ctor for Vec

// solution 1: implement a default ctor for Vec
...
// solution 2: implement a default ctor for Plane
Plane::Plane() : v1(0,0,0), v2(1, 1, 1) {}
Plane p;      // now this will compile
```

Embedding an object (Vec) inside another (Plane) is composition.
A plane "owns" a Vec.
Typically A "owns" a B if 

- B does not exist outside A
- if A is destroyed, so is B
- if A is copied, so is B (deep copy)  
To illustrate, a car owns some wheels.

Using graphs to represent compositions (hard here...)

```
...... (shaded diamond ----->)
```

### Aggregation "has a"

Parts in a catalog, Catalog has a parts
Typically, A "has a" B if 
- B can exist outside A
- A is destoryed, B is not
- A is copied, B is not (shallow copy)

Using graphs
```
...... (open diamond ----->)
```

Class Catalog {
	Part *parts;
};


### Inheritance

```
|______Book_______|
|  - title        |
|  - autor        |
|  - numPage      |


|____TextBook_____|
|  - title        |
|  - author       |
|  - numPages     |
|  - topic        |


|____Comic________|
|  - title        |
|  - author       |
|  - numPages     |
|  - protag       |
```

In C, to represent a collection of books
1. array of void *
2. union keyword
   (create a union book type)
	 union BookType {
		 Book *b;
		 TextBook *t;
		 Comic *x;
	 };

In C++, we use the "is a" relationship.
TextBook is a Book, Comic is a Book.

```
|______Book_______|
|  - title        |
|  - autor        |
|  - numPage      |
|_________________|
```

Book is a Base Class, or a super Class.
TextBook and Comic are derived classes or subclasses

```c++
Class Book {
	string title, author;
	int numPages;
	public: 
	Book(string title, string author, int numPages) ... {}
};

class TextBook : public Book {
	string topic;
}

class Comic : public Book {
	string protag;
};
```

A TextBook object reserves space for titile, author, numPages, and topic.

