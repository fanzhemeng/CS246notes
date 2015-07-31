# Lecture 24 Exceptions

will be one question on the final.

Note: Book a demo time slot. If you forget or do not show up, you get 0 for correctness.

Recap:

```c++
Book *b2 = ...;
TextBook *ptb = dynamic_cast<TextBook *>(b2);  // return NULL if fail, NICE
if (ptb) cout << "Its a TextBook" << endl;
else cout << "Not a TextBook" << endl;
```

Dynamic cast with References
```c++
TextBook t(..);
Book &b = t;
TextBook &t1 = dynamic_cast<TextBook &> (b);
```

If b is a reference to a TextBook, then t1 will become a reference to the same TextBook.
However, **References cannot be NULL.**
A bad_cast error (exception) occurs if b is not a reference to a TextBook.

Other examples:

- when new fails (no heap memory available): bad_alloc
- when the index used for vector::at is out of range: out_of_range

O_o c++/exceptions/[rangeError.cc, rangeErrorCaught.cc, callchain.cc]

By default, C++ programs terminate if an exception is thrown / raised.

When an exception occurs, the call stack is repeatedly popped until an appropriate handler (a catch block) is found.
This is *stack unwinding*.

When using exception in your programs, you should document it properly so that others who use it can implement handler accordingly. 
When you are using others programs that potentially will throw some excsptions, read their documentation carefully and implement your own handler.

Error Recovering can be a multi-step process.
example: 

```c++
try {
	... // throw TextBook
}
catch (someException s) {  // Book s
	partly recover;
	throw someOtherException(..);
}
```

Alternatively, 

```c++
throw s;
throw;   // re-throws what was just caught
```

**ADVICE**: catch by reference to prevent slicing and copying.

Moreover, the ordering of the catch blocks matters. An exception will be handled by the first catch block that is able to handle it. 
So you propably want to put the subclasses exception handler in front of the super classes (genenral handlers) when ordering.

All C++ library exceptions inherit from a dase "exception" class.
However, in C++ you can throw anthing (such as int, etc. ).

Syntax for catching all exceptions:

```c++
try {
	...
} catch (...) {  // this 'dot dot dot' really represents anything
	...
}
```


```c++
Book *p = new TextBook(..);
Book *q = new TextBook(..);
*p = *q;  // assignment operator, not virtual, so only fields of Book will be assigned because p and q are Book *

TextBook &TextBook::operator=(const Book &other) {
	TextBook &tother = dynamic_cast<TextBook &>(other);
	Book::operator=(other);
	topic = tother.topic;   // should not use other because other is of type Book &
	return *this;
}
```

Then in your code when you use TextBook::operator=, you should provide handler for the exception for bad_cast.


Exceptions are a game changer.

```c++
void f() {
	MyClass *p = new MyClass;
	MyClass q;
	...
	g();  // throw an exception
	...
	delete p;  // possible for this not to be executed, if there is an exception thrown above, and no handler for it
}
```

if g() throws, p is never deleted. We leak memory.
We want to write code that is exception safe.

C++ Guarantee: the destructor for stack allocated objects are always called. (even during stack unwinding due to an exception)

This is why we do not worry about q in the above function.


Exception safe code:

```c++
void f() {
	MyClass *p = new MyClass;
	MyClass q;
	try {
		g();
	} catch (...) {
		delete p;
		throw;
	}
	delete p;
}
```

**ADVICE**: good idea to use stack when you can.

**ADVICE2**: never let a destructor throw an exception.

When your program have two exceptions at the same time, it will terminate immediately no matter what. Because C++ does not allow that.

C++ idiom: RAII
Resource Acquisition is initializetion.
That is, every resource should be wrapped in a stack allocated object. (whose job is to reclaim the resources)

```
ifstream f("file");   // file is a resource
```

C++ provides a templated class auto-ptr<T>.
