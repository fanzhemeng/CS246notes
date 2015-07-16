# Lecture 20: Design Pattern

Concret observer calls concret subject attach. 

- attach stores a pointer to the concret observer.

An event occurs in CS.
CS.notifyobserver, which is AO::notify
CO calls CS getstate.

Rule of Thumb: if a class needs to be abstract, but there is no clear pure virtual method, **make the destructor pure virtual**.

Another Rule: a class must always have a destructor.

Pure Virtual: can have an implementation, but the subclasses must implement the pure virtual method to be concret.

```c++
class Component {
	public:
	void render() = 0;
};

class Win : public Component {
	public: 
	coid render() { ...   // display window }
};

class DecWin : public Component {
	public: 
	Component *uniderlyingWin;
	// void reder() = 0;
};

class ScrollWin : public DecWin {
	public: 
	ScrollWin(Component *w) : underlying(w) {}
	void render() {
		underlyingWin->render();

		// code to display the scrollbar

	}
};

Component *c = new Win;
c = new ScrollWin(c);
```

## Design Pattern: Factory Method Pattern

Suppose we want to create a game. Have an abstract class Enemy, which has subclasses Turtle and Bullet.
Another abstract class Level, which has subclasses Normal and Castle.

```c++
class Level {
	public: 
	virtual Enemy *createEnemy() = 0;
};

class Normal : public Level {
	public: 
	Enemy *createEnemy() { more turtles}
};


Player *p = ...;
Level *l = ...;
while (p->notDead()) {
	// generate an enemy
	Enemy *e = l->createEnemy();  // factory
	// attack player
}
```

Factory pattern often works well with the singleton pattern.

Now we want have two subclasses of Turtle, called RedTurtle and GreenTurtle.

```c++
class Turtle ... {
	public:
	void draw() {
		drawHead();
		drawShell();
		drawTail();
	}
	private:
	void drawHead() {...}
	void drawTail() {...}
	virtual drawShell() = 0;
};
```

