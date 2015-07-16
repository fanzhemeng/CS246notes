# Lecture 18 

##Last Time:
System modelling ( composition, aggregation, inheritance)

```c++
class Book {
	string title, author;
	int numPages;
	public:
	Book(string title, string author, int numPages)
		: title(title), author(author), numPages(numPages) {}
};

class Comic : public Book {
	string protag;
};

class TextBook : public Book {
	string topic;
	public:
};
```

Private really does mean private

- TextBook and Comic cannot access (read or write) the Book Fields

```c++
   TextBook tb = ...;
	 tb.title = ...;              // cannot do this
	 cout << tb.title << endl;    // or this

	 TextBook::TextBook(.., .., ..) 
	   : title(title), author(author), numPages(numPages), topic(topic) {}

	 the ctor above will not compile
	  - inherited fields are private
	  Steps that occur when an object is created
		1. space is allocated
		2. superclass part is initialized
		3. fields initialization
		4. the ctor body runs

	  - Book does not have a default ctor that is needed for step 2 of object creation

    solution:
		TextBook::TextBook(string title, string author, int numPages, string topic)
		  : Book(title, author, numPages), topic(topic) {}
```


## Protected Keyword
-  `-` private - only class
-  `+` public - everyone
-  `#` protected - only class and some classes

```c++
class Book {
	protected:
	string title, author;
	int numPages;
};

class TextBook : public Book {
	string topic;
	public:
	void setAuthor(string auth) {
		author += auth;
	}
};

int main() {
	TextBook tb(..);
	tb.setAuthor("Nomair");     // can do this
	tb.author = ...;            // cannot do this
	return 0;
}
```

## Motivation Virtual

- Book is heavy if > 200
- TextBook is heavy if > 400
- Comic is heavy if > 30

```c++
class Book {
	protected:
	int numPages;
	public: 
	bool isItHeavy() {return numPages > 200; }
}

class TextBook : public Book {
	public:
	bool isItHeavy() {return numPages > 400; }
	// method overloading
};

class Comic : public Book {
	public:
	bool isItHeavy() { return numPages > 30; }
};

Book b(.., .., 50);
Comic cb(..,..,40, ..);
b.isItHeavy();           // Book::isItHeavy runs
cb.isItHeavy();          // Comic::isItHeavy runs

Book b2 = Comic(..,..,40,..);   // can do this because Comic is a Book
b2.isItHeavy();          // Book::isItHeavy runs
```

Compiler looks at the declared type of b2
Object slicing.  protag is thrown away when doing it.

```c++
Comic cb(..,..,40,..);
Book *p = &cb;
Comic *cp = &cb;
cp->isItHeavy();           // Comic::isItHeavy runs
p->isItHeavy();            // Book::isItHeavy runs
```

If we access an object through a superclass pointer, then the
superclass method will execute.

The same object, cb, acts differently depending on the pointer used to 
access it.
The compiler uses the declared type of the pointer to choose which method
to run.

We would like to refer to different Books to using superclass (Book)
pointers, and still have the specialized methods execute.

Declare the method virtual. only use the keyword once.
```c++
virtual bool isItHeavy() const;
```

Note that the virtual keyword applies when we have pointers to object.
When you have a virtual method called, the compiler does not make the 
decision of which method to call based on the declared type of the 
pointer. Instead, the decision will be made based on the type of the 
object pointed to at runtime.
This is called dynamic dispatch.

```c++
Book *collection[20];    // polymorphic array of Book pointers
for (int i = 0; i < 20; i ++) {
	collection[i]->isItHeavy();
}
```

Dynamic dispatch incurs a small cost.

Every method in jave uses dynamic dispatch impilicitly.

Polymorphism: Collection array accomodates multiple types under 
one abstraction (array)

