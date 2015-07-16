# Lecture 19: Dtor revisited, pure virtual, observer pattern, Decorator

Last time: virtual polymorphism

## Destructor
The three things that happen when an object is destroyed:

- destructor body runs
- destructor for fields run in reverse declaration order
- space is reclaimed

Always make the base class dtor virtual to avoid memory leak.

With virtual keyword, and call delete on a object of subclass, the 
base class dtor will be called between step 2 and 3 in the 
procedure above, and do it recursively with the 3 steps.


## Pure Virtual
(see picture) 

```c++
class Student {
	protected:
	int numCourses;
	public:
	virtual int fees() = 0;     // pure virtual method, method with no implementation
}
```

student class is incomplete because of the pure virtual method.
So `Student s;` will not compile, because cannot create objects that have even one pure virtual method.
Student is an example of an **abstract** class.

But we can use the Student class by using its pointer.
Abstract class 
- can be used to organize entities
- share common fields / methods
- Polymorphism
  for example, `Student *student[100];  student[i]->fees();`

A derived class inherits pure virtual mthods from the base class.
The derived class must implement all pure virtual methods to be considered non abstract, or **concrete**.

A class is considered to be abstract if it has at least one pure virtual method, and have as many as common methods and virtual methods as you want.
A abstact class should also have its constructor, since it will be called when a subclass object is created.

In UML, abstract class name, pure virtual methods, and virtual methods are in *italics*.

```c++
class Regular : public Student {
	public: 
	int fees() {
		return numCOurses * 123456789;
	}
};
```

## Obsever Pattern
- Publish / subscribe system

```
Subject                                   Observer
- generates data                         - interested in the data
- provide a subscribe method             - provide a notify method
```

example: se/2-../observer


## Decorator Pattern
Decorator is a component.
Decorator has a (or, owns a) component.

I want a plain window with a scroll and menu.

```c++
Component *c : new PlainWindow;
c = new Scroll(c);
c= new Menu(c);
```
