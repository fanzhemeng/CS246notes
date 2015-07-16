# Lecture 21  C++ Templates, STL, Visitor Design Pattern

Last Time: Factory and Template Method Pattern

## Template

```c++
class Node {
	int data;
	Node *next;
	// methods omitted
};
```
Now we can create linked list using Node.
What if we want other data type?

We can create other Node class for other data types.
Or we can use template.
Template: parameterize a class on one or more type variables.
We want to replace the int type in class Node with a type variable.
So the Node class will look like the following: 

```c++
template <typename T> class Node {
	T data;
	Node<T> *next;   // Node<T> or Node <T> are both OK
	public:
	Node(T data, Node<T> *next) : data(data), next(next) {}
	T getData() const { return data; }
	Node<T> *getNext() const { return next; }
};

Node<int> *i = new Node<int>(1, new Node<int>(2, new Node<int>(3, NULL)));
Node<char> *c = new Node<char>('a', new Node<char>('b', new Node<char>('c', NULL)));
```

When you implement a template, the compiler copy the class def, replace the typename with
the actual type, and then compile this copy. This is why it is called template.

```c++
Node< Node<int> > *listofList;  // note a space between > and >, only for C++03, fixed in C++11
```

Know the format of a template for final, not required to implement one.


## Standard Template Libaray (STL)

- Vector
- Map

### Vector
Vector: dynamically length arrays

- vector will internally resize as needed
  std::string is implemented by vectors
- parameterized on one type 

```c++
#include <vector>

int main() {
	vector<int> vec;
	vec.push_back(22);
	vec.push_back(42);
	for (int i = 0; i < vec.size(); i ++) {
		cout << vec[i] << endl;         // range unchecked
		// cout << vec.at(i) << endl;   // range checked
		vec.pop_back();                 // remove the last element
	}
	return 0;
}
```

The cool kids in CS136 did this for interating through an array (using pointers):
```c++
for (int *p = v; p < v + size; p ++) {  // v is an array
	cout << *p << endl;
}
```

Now we want to iterating through a vector. The super cool CS246 kids use STL iterators

```c++
for (vector<int>::iterator i = vec.begin(); i != vec.end(); i ++) {
	cout << *i << endl;
}
```

Just past the memory used by a vector. An iterator is an abstraction of pointers.
We can also go through a vector in reverse order, from back to front.

```c++
for (vector<int>::reverse-iterator rit = vec.rbegin(); rit != vec.rend(); rit ++) {
	cout << *rit << endl;
}
```
Note that v.begin() is the start of array, v.end() is one size after last element of array; 
v.rbegin() is the last element in the array, v.rend() is one size before start of array.
(both begin mehthods are in the array, both end methods not)

Note that when increment iterator by > 1 each, should check if it goes out of array in the for body.

```c++
vec.erase(vec.begin());
vec.erase(vec.rbegin());   // same as vec.erase(vec.end() - 1)
```

### STL: map
array: int (key) --> AnyType (value)

Map allows any type for the (key, vlaue) pair.

```c++
#include <map>

int main() {
	map<string, int> m;
	m["abc"] = 1;
	m["def"] = 2;
	cout << m["def"] << endl;
	if (m.count("abc")) { // return 1 if "abc" is found, 0 otherwise
		cout << "found abc" << endl;
	}
	for (map<string, int>::iterator mit = ...; ..; ..) {   // the map iterator goes through the map in sorted key order
		(*mit).first    // gives the key
		(*mit).second   // gives the value
	}
}
```

Map iterator goes through the map in sorted key order.  map orders are implemented by buckets (hash table?).
We can look up more for maps, multimaps, etc.


## Visitor Design Pattern
Dynamic Dispatch: the ability to decide on which method to call based on the runtime type of the object.

Double Dispatch: choose a method to run based on the funtime type of two objects.

An abstract class Enemy has two subclasses, turtle and bullet.
Class Weapon has two subclasses, Stick and Rock.
So we get 4 different behaviour when we attack enemy using weapon. We want double dispatch.

```c++
Enemy *e = ..;
Weapon *w = ..;
```
