# Object oriented Programming Q/A

## important points
---
* A constructor without any arguments or with the default value for every argument is said to be the **Default constructor**.

* Pass-by-references is more efficient than pass-by-value, because it does not copy the arguments. The formal parameter is an alias for the argument.

* one of the solid principles to adhere to in sw engineering is the **single responsibility principle**. for example if you have multiple classes that contain multiple entries inside it and you want the ability to save each object's entries into a pdf file, what you want to do is to create another class for example ```pdfManager``` and you can add the code that recieve, convert and save any type of the previous classes' objects to a pdf files. why do that in ```pdfManager```?  cause it's not the classes responsibility to do that, it's better to create new class that is only responsible for this kind of things and do these services to other classes.

* **virtual functions** are called according to the type of the object instance pointed to or referenced, not according to the type of the pointer or reference.
In other words, virtual functions are resolved late, at runtime. This technique falls under Runtime Polymorphism.

* java keeps all methods virtual by default

* Sometimes implementation of all function cannot be provided in a base class because we don’t know the implementation. Such a class is called **abstract class**.

* **abstract classes** cannot be used to create objects, this will lead to compiler error.

* A class is abstract if it has at least one pure virtual function.

* **pure virtual functions** play their role for creating abstract classes but they also fulfill their duty of runtime polymorphism.

* We can have pointers and references of abstract class type. that's why runtime polymorphism still works with 'em.

* If we do not override the pure virtual function in derived class, then derived class also becomes abstract class.

* ```inline``` and ```explicit``` is the only keyword that is allowed in declaring constructors. they cannot be static, virtual or const.

* An abstract class can have constructors (for derived classes to use).
```c
#include<iostream>
using namespace std;
  
// An abstract class with constructor
class Base
{
protected:
int x;
public:
virtual void fun() = 0;
Base(int i) {
              x = i; 
            cout<<"Constructor of base called\n";
            }
};
  
class Derived: public Base
{
    int y;
public:
    Derived(int i, int j):Base(i) { y = j; }
    void fun() { cout << "x = " << x << ", y = " << y<<'\n'; }
};
  
int main(void)
{ 
    Derived d(4, 5); 
    d.fun();
    
  //object creation using pointer of base class
    Base *ptr=new Derived(6,7);
      ptr->fun();
    return 0;
}
```
```
Constructor of base called
x = 4, y = 5
Constructor of base called
x = 6, y = 7
```

* the only difference between ```struct``` and ```class``` in cpp is that ```struct`` is public by default.

* In Java, a class can be made abstract by using abstract keyword. Similarly a function can be made pure virtual or abstract by using abstract keyword.

* **static data members** have Only one copy of that member is created for the entire class and is shared by all the objects of that class and It is initialized before any object of this class is created, even before the main starts. Also it is visible only within the class, but its lifetime is the entire program

* **Static members** are only declared in a class declaration, not defined. They must be explicitly defined outside the class using the scope resolution operator.

* Static data members can only be defined globally in C++. The only exception to this are static const data members of integral type which can be initialized in the class declaration.

* static member functions do not have **this** pointer.

* A static member function cannot be virtual

* Member function declarations with the same name and the name parameter-type-list cannot be overloaded if any of them is a static member function declaration. 

* A static member function can not be declared ```const```, ```volatile```, or ```const volatile```.

* **friends of your parent is not friends of your**

* **Data abstraction** refers to providing only essential information about the data to the outside world, hiding the background details or implementation. 

* Two Important  property of Encapsulation 

  * Data Protection: Encapsulation protects the internal state of an object by keeping its data members private. Access to and modification of these data members is restricted to the class’s public methods, ensuring controlled and secure data manipulation.
  * Information Hiding: Encapsulation hides the internal implementation details of a class from external code. Only the public interface of the class is accessible, providing abstraction and simplifying the usage of the class while allowing the internal implementation to be modified without impacting external code.

## Questions
---
> Is a default constructor automatically provided?

If no constructors are explicitly declared in the class, a default constructor is provided automatically by the compiler. 

> Does compiler create default constructor
when we write our own?

short answer, NO

> When are static objects destroyed?

static objects are always destroyed at the end of the program whether their scope is local or global. This property allows them to retain their value between multiple function calls and we can even use a pointer to them to access them out of scope.

> when static global objects gets constructed?

constructed even before the main.

> Can a default constructor contain a default argument?

Yes, a constructor can contain default argument with default values for an object.

> Will there be any code inserted by the compiler to the user implemented default constructor behind the scenes?

The compiler will implicitly declare the default constructor if not provided by the programmer, will define it when in need. The compiler-defined default constructor is required to do certain initialization of class internals. It will not touch the data members or plain old data types (aggregates like an array, structures, etc…). However, the compiler generates code for the default constructor based on the situation.

Consider a class derived from another class with the default constructor, or a class containing another class object with the default constructor. The compiler needs to insert code to call the default constructors of the base class/embedded object.

> What is the use of **private destructor**?

if we declare the destructor to be private, locally temporary objects cannot be created, it will result on compiler error because calling the destructor while it's private won't pass the compiler. on the other hand, dynamically allocated objects is allowed as it does not call the destructor implicitly, you need to call it yourself by attempting to delete it. of course No ordinary function can delete this object for you so it needs to be a friend function or class that will be responsible of deleting this objects and all the objects of it's class.

```c
class Test {
private:
    ~Test() {}
};
 
// Driver Code
int main()
{
    Test* t = new Test;
    delete t;       // WILL RESULT ON ERROR
}
```

so the role of destructing the objects of this class is restricted to other friend function or class.
```c
// A class with private destructor
class Test {
private:
    ~Test() {}
 
public:
    friend void destructTest(Test*);
};
 
// Only this function can destruct objects of Test
void destructTest(Test* ptr) { delete ptr; }
 
int main()
{
    // create an object
    Test* ptr = new Test;
 
    // destruct the object
    destructTest(ptr);
 
    return 0;
}
```
we can tolerate this without using any friends
```c
class parent {
    // private destructor
    ~parent() { cout << "destructor called" << endl; }
 
public:
    parent() { cout << "constructor called" << endl; }
    void destruct() { delete this; }
};
 
int main()
{
    parent* p;
    p = new parent;
    // destructor called
    p->destruct();
 
    return 0;
}
```

> How constructors are different from a normal member function?

* Constructor has same name as the class itself
* Default Constructors don’t have input argument however, Copy and Parameterized Constructors have input arguments
* Constructors don’t have return type
* A constructor is automatically called when an object is created.
* It must be placed in public section of class.
* If we do not specify a constructor, C++ compiler generates a default constructor for object (expects no parameters and has an empty body).

> what do you know about constructors?

* The name of the constructor is the same as its class name.
* Constructors are mostly declared in the public section of the class though it can be declared in the private section of the class.
* Constructors do not return values; hence they do not have a return type.
* A constructor gets called automatically when we create the object of the class.
* Constructors can be overloaded.
* Constructor can not be declared virtual.
* Constructor cannot be inherited.
* Addresses of Constructor cannot be referred.
* Constructor make implicit calls to new and delete operators during memory allocation.

> Uses of Parameterized constructor? 

* It is used to initialize the various data elements of different objects with different values when they are created.
* It is used to overload constructors


> What are the functions that are generated by the compiler by default, if we do not provide them explicitly?

The functions that are generated by the compiler by default if we do not provide them explicitly are:
* Default constructor
* Copy constructor
* Assignment operator
* Destructor

> what do you know about destructors?

1. Destructor is invoked automatically by the compiler when its corresponding constructor goes out of scope and releases the memory space that is no longer required by the program.
2. Destructor neither requires any argument nor returns any value therefore it cannot be overloaded.
3. Destructor  cannot be declared as static and const;
4. Destructor should be declared in the public section of the program.
5. Destructor is called in the reverse order of its constructor invocation.

> can we define **private constructors**?

Yes, it can be used in these cases:
1. If we want that class should not be instantiated by anyone else but only by a friend class.
2. Using **Singleton design pattern**: When we want to design a singleton class. This means instead of creating several objects of class, the system is driven by a single object or a very limited number of objects.
3. **Named Constructor Idiom** : Since constructor has same name as of class, different constructors are differentiated by their parameter list, but if numbers of constructors is more, then implementation can become error prone. 
With the Named Constructor Idiom, you declare all the class’s constructors in the private or protected sections, and then for accessing objects of class, you create public static functions.

```c
class Point 
{
    public:
      
    // Rectangular coordinates
    Point(float x, float y);     
      
    // Polar coordinates (radius and angle)
    Point(float r, float a);     
      
    // error: ‘Point::Point(float, float)’ cannot
    // be overloaded
};
int main()
{
    // Ambiguous: Which constructor to be called ?
    Point p = Point(5.7, 1.2); 
    return 0;
}
```
This problem can be resolved by Named Constructor Idiom.
```c
// CPP program to demonstrate
// named constructor idiom
#include <iostream>
#include <cmath>
using namespace std;
class Point
{
	private:
	float x1, y1;
	Point(float x, float y)
	{
		x1 = x;
		y1 = y;
	};
public:
	// polar(radius, angle)
	static Point Polar(float, float);
	
	// rectangular(x, y)
	static Point Rectangular(float, float);
	void display();
};

// utility function for displaying of coordinates
void Point :: display()
{
	cout << "x :: " << this->x1 <<endl;
	cout << "y :: " << this->y1 <<endl;
}

// return polar coordinates
Point Point :: Polar(float x, float y)
{
	return Point(x*cos(y), x*sin(y));
}

// return rectangular coordinates
Point Point :: Rectangular(float x, float y)
{
	return Point(x,y);
}
int main()
{
	// Polar coordinates
	Point pp = Point::Polar(5.7, 1.2);
	cout << "polar coordinates \n";
	pp.display();
	
	// rectangular coordinates
	Point pr = Point::Rectangular(5.7,1.2);
	cout << "rectangular coordinates \n";
	pr.display();
	return 0;
}

```

> is it possible to access private members outside a class without friend?

Yes, it is possible using pointers. Although it’s a loophole in C++, yes it’s possible through pointers.

```c
class Test {
private:
    int data;
 
public:
    Test() { data = 0; }
    int getData() { return data; }
};
 
int main()
{
    Test t;
    int* ptr = (int*)&t;
    *ptr = 10;
    cout << t.getData();
    return 0;
}
```

> what is RVO?

RVO basically means the compiler is allowed to avoid creating temporary objects for return values, even if they have side effects. for example
```c
struct Snitch {   // Note: All methods have side effects
  Snitch() { cout << "c'tor" << endl; }
  ~Snitch() { cout << "d'tor" << endl; }

  Snitch(const Snitch&) { cout << "copy c'tor" << endl; }
  Snitch(Snitch&&) { cout << "move c'tor" << endl; }

  Snitch& operator=(const Snitch&) {
    cout << "copy assignment" << endl;
    return *this;
  }

  Snitch& operator=(Snitch&&) {
    cout << "move assignment" << endl;
    return *this;
  }
};

Snitch ExampleRVO() {
  return Snitch();
}

int main() {
  Snitch snitch = ExampleRVO();
}
```
```
$ clang++ -std=c++11 main.cpp && ./a.out
c'tor
d'tor

$ clang++ -std=c++11 -fno-elide-constructors main.cpp && ./a.out
c'tor
move c'tor
d'tor
move c'tor
d'tor
d'tor
```

Without RVO the compiler creates 3 Snitch objects instead of 1:

1. A temporary object inside ```ExampleRVO()``` (when printing ```c'tor```);
2. A temporary object for the returned object inside ```main()``` (when printing the first ```move c'tor```);
3. The named object ```snitch``` (when printing the second move ```c'tor```).

> how return value optimization really work?

The neat thing about RVO is that it makes returning objects free. It works via allocating memory for the to-be-returned object in the caller’s stack frame. The returning function then uses that memory as if it was in its own frame without the programmer knowing / caring.

> what is NRVO?

Named RVO is when an object with a name is returned but is nevertheless not copied. A simple example is:
```c
Snitch ExampleNRVO() {
  Snitch snitch;
  return snitch;
}

int main() {
  ExampleNRVO();
}
```
```
c'tor
d'tor
```

> what is copy-elision?

group of optimization including RVO and NRVO, except it's not required to happen as part of return statements like RVO and NRVO.
```c
void foo(Snitch s) {
}

int main() {
  foo(Snitch());
}
```
```
c'tor
d'tor
```
RVO is more frequent (and thus useful) than other copy-elision practices.

> When RVO doesn’t / can’t happen?

* Deciding on Instance at Runtime
```c
Snitch CreateSnitch(bool runtime_condition) {
  Snitch a, b;
  if (runtime_condition) {
    return a;
  } else {
    return b;
  }
}

int main() {
  Snitch snitch = CreateSnitch(true);
}
```

* Assignment

RVO can only happen when an object is created from a returned value. Using operator= on an existing object rather than copy/move constructor might be mistakenly thought of as RVO, but it isn’t:
```c
Snitch CreateSnitch() {
  return Snitch();
}

int main() {
  Snitch s = CreateSnitch();    //RVO applied
  s = CreateSnitch();           //RVO not applied
}
```
```
c'tor
c'tor
move assignment
d'tor
d'tor
```

* Returning a Parameter / Global

When returning an object that is not created in the scope of the function there is no way to do RVO:
```c
Snitch global_snitch;

Snitch ReturnParameter(Snitch snitch) {
  return snitch;
}

Snitch ReturnGlobal() {
  return global_snitch;
}

int main() {
  Snitch snitch = ReturnParameter(global_snitch);
  Snitch snitch2 = ReturnGlobal();
}
```
```
c'tor
copy c'tor
move c'tor
d'tor
copy c'tor
d'tor
d'tor
d'tor
```
[learn More](https://shaharmike.com/cpp/rvo/)

> What is the use of **private destructor**?

Whenever we want to control the destruction of objects of a class, we make the destructor private. For dynamically created objects, it may happen that you pass a pointer to the object to a function and the function deletes the object. If the object is referred after the function call, the reference will become dangling.

this code gives a compiler error
```c
class Test {
private:
    ~Test() {}
};
int main() { Test t; }
```

so as this
```c
class Test {
private:
    ~Test() {}
};
 
// Driver Code
int main()
{
    Test* t = new Test;
    delete t;
}
```


Two ways to delete the obj intentionally
```c
// first way
class Test {
private:
    ~Test() {}
 
public:
    friend void destructTest(Test*);
};
 
// Only this function can destruct objects of Test
void destructTest(Test* ptr) { delete ptr; }
 
int main()
{
    // create an object
    Test* ptr = new Test;
 
    // destruct the object
    destructTest(ptr);
 
    return 0;
}

//second way
class parent {
    // private destructor
    ~parent() { cout << "destructor called" << endl; }
 
public:
    parent() { cout << "constructor called" << endl; }
    void destruct() { delete this; }
};
 
int main()
{
    parent* p;
    p = new parent;
    // destructor called
    p->destruct();
 
    return 0;
}
```

> what is the **explicit** keyword?

Explicit Keyword in C++ is used to mark constructors to not implicitly convert types in C++. It is optional for constructors that take exactly one argument and work on constructors(with a single argument) since those are the only constructors that can be used in typecasting. We can avoid such implicit conversions as these may lead to unexpected results.

> do we need to declare virtual functions by virtual keyword for each child of the base?

In C++, once a member function is declared as a virtual function in a base class, it becomes virtual in every class derived from that base class. In other words, it is not necessary to use the keyword virtual in the derived class while declaring redefined versions of the virtual base class function.

```c

// C++ Program to demonstrate Virtual
// functions in derived classes
#include <iostream>
using namespace std;
 
class A {
public:
    virtual void fun() { cout << "\n A::fun() called "; }
};
 
class B : public A {
public:
    void fun() { cout << "\n B::fun() called "; }
};
 
class C : public B {
public:
    void fun() { cout << "\n C::fun() called "; }
};
 
int main()
{
    // An object of class C
    C c;
   
    // A pointer of class B pointing
    // to memory location of c
    B* b = &c;
   
    // this line prints "C::fun() called"
    b->fun();
   
    getchar(); // to get the next character
    return 0;
}
```

> Can we make a class constructor virtual in C++ to create polymorphic objects?

No. C++ being a statically typed (the purpose of RTTI is different) language, it is meaningless to the C++ compiler to create an object polymorphically. The compiler must be aware of the class type to create the object. In other words, what type of object to be created is a compile-time decision from the C++ compiler perspective. If we make a constructor virtual, the compiler flags an error. In fact, except inline, no other keyword is allowed in the declaration of the constructor.

> How can we create the required type of an object at runtime?

by using the factory method. based on user's input, it will return a specific object type of the base or derived objs of it.

[learn more...](https://www.geeksforgeeks.org/advanced-c-virtual-constructor/)


> What happens when we write only a copy constructor – does the compiler create a default constructor?
 
The compiler doesn’t create a default constructor if we write any constructor even if it is a copy constructor. On the other hand, The compiler creates a copy constructor if we don’t write our own. The compiler creates it even if we have written other constructors in a class

> when we need to create our own copy constructor?

we need to write a copy constructor only when we have pointers or run-time allocation of resources like filehandle, a network connection, etc.

> Why copy constructor argument should be const in C++? why does this program produces a compiler error actually?
```c
class Test
{
   /* Class data members */
public:
   Test(Test &t) { /* Copy data members from t*/}
   Test()        { /* Initialize data members */ }
};
 
Test fun()
{
    cout << "fun() Called\n";
    Test t;
    return t;
}
 
int main()
{
    Test t1;
    Test t2 = fun();
    return 0;
}
```

It gets executed but copy constructor is not called, instead it calls the default constructor where assignment operator is overloaded. Even if we have an explicit overloaded assignment operator, it is not going to call it. 

The function fun() returns by value. So the compiler creates a temporary object which is copied to t2 using copy constructor in the original program (The temporary object is passed as an argument to copy constructor). The reason for compiler error is, compiler created temporary objects cannot be bound to non-const references and the original program tries to do that. It doesn’t make sense to modify compiler created temporary objects as they can die any moment. 


> tell me a use case of the virtual functions?

Virtual functions allow us to create a list of base class pointers and call methods of any of the derived classes without even knowing the kind of derived class object. 

> what is the difference between interface and abstract classes and how to implement it in cpp and java?

An interface does not have implementation of any of its methods, it can be considered as a collection of method declarations. In C++, an interface can be simulated by making all methods as pure virtual. In Java, there is a separate keyword for interface.


> difference between directed association and dependency in UML

An association almost always implies that one object has the other object as a field/property/attribute (terminology differs). A dependency typically (but not always) implies that an object accepts another object as a method parameter, instantiates, or uses another object. In OOP terms:

* Association --> **A has-a C** object (as a member variable)

* Dependency --> **A references B** (as a method parameter or return type)

> Should dependencies in UML have multiplicities shown

No, there's no point in multiplicity on a dependency. Dependency merely states that a classifier (usually a class) in some way depends on another classifier. There is no way to say you depend on specific amount of those other classifiers since it doesn't touch the instances level.

> does static in anyway permit in-class initialization?

No, it cannot be permitted unless static is aligned with ```inline``` keyword which is supported starting cpp17.

> if you have a ```static const``` data member in your class, can you init it inside the class?

No, any static data member must be defined outside the class in a global scope, if you insist you can use inline or make it **non static** to be able to define it inside the class.

> can we initialize private static data members outside the class?

Totally legal.

> can virtual functions be private?

yes it can be private as C++ has access control, but not visibility control. As mentioned virtual functions can be overridden by the derived class but under all circumstances will only be called within the base class.

```c
// C++ program to demonstrate how a
// virtual function can be private
#include <iostream>

class base {
public:
	// default base constructor
	base() { std::cout << "base class constructor\n"; }

	// virtual base destructor
	// always use virtual base
	// destructors when you know you
	// will inherit this class
	virtual ~base()
	{
		std::cout << "base class destructor\n";
	}
	// public method in base class
	void show()
	{
		std::cout << "show() called on base class\n";
	}

	// public virtual function in base class,
	// contents of this function are printed when called
	// with base class object when called with base class
	// pointer contents of derived class are printed on
	// screen
	virtual void print()
	{
		std::cout << "print() called on base class\n";
	}
};

class derived : public base {
public:
	// default derived constructor
	derived()
		: base()
	{
		std::cout << "derived class constructor\n";
	}
	// virtual derived destructor
	// always use virtual destructors
	// when inheriting from a
	// base class
	virtual ~derived()
	{
		std::cout << "derived class destructor\n";
	}

private:
	// private virtual function in derived class overwrites
	// base class virtual method contents of this function
	// are printed when called with base class pointer or
	// when called with derived class object
	virtual void print()
	{
		std::cout << "print() called on derived class\n";
	}
};

int main()
{
	std::cout << "printing with base class pointer\n";

	// construct base class pointer with derived class
	// memory
	base* b_ptr = new derived();

	// call base class show()
	b_ptr->show();

	// call virtual print in base class but it is overridden
	// in derived class also note that print() is private
	// member in derived class, still the contents of
	// derived class are printed this code works because
	// base class defines a public interface and drived
	// class overrides it in its implementation
	b_ptr->print();

	delete b_ptr;
}
```

> Why is the size of an empty class not zero?

to be recognizable and have a unique address in the memory, otherwise how you would refer to it in the program if it doesn't consume memory and don't have an address.

> can static methods access object members?

NO, it can only access the static members of the class, even if it's called from an object, it cannot access it's members.

> Can a C++ class have an object of self type?

A class declaration can contain static object of self type, it can also have pointer to self type, but it cannot have a non-static object of self type. If a non-static object is member then declaration of class is incomplete and compiler has no way to find out size of the objects of the class.
Static variables do not contribute to the size of objects. So no problem in calculating size with static variables of self type.
For a compiler, all pointers have a fixed size irrespective of the data type they are pointing to, so no problem with this also.

> can we make virtual functions having typenames of templates in c++?

No, you cannot declare a virtual function template in C++. 

A virtual function in C++ enables polymorphism, allowing a subclass to provide a different implementation for a function that is already provided by its superclass. This is done via a mechanism known as dynamic dispatch (or runtime polymorphism). At runtime, the machine will look at what is the actual type of the object and call the appropriate method accordingly. 

A function template, on the other hand, is a blueprint for creating functions. Each time you use the function with a different type, the compiler creates a new version (an instantiation) of the function specifically for that type. This is done at compile-time (static polymorphism). 

Combining these two mechanisms is not possible because virtual functions rely on runtime type information, while templates are instantiated at compile time.

If you need a function to operate on multiple types and also support polymorphism, consider alternatives like:

1. Using a base class and virtual functions without templates. This requires that the types you want to work with all share a common base class.
2. Using templates without virtual functions. This doesn't support runtime polymorphism but can handle multiple types.
3. Using type erasure techniques, like those employed in `std::function`, to make compile-time polymorphism (via templates) look like runtime polymorphism. Note that this is an advanced topic and typically requires a good understanding of C++ templates and memory management.


## stackoverflow Q/A
---

### Reference member variables as class members

In my place of work I see this style used extensively:-

```c
#include <iostream>

using namespace std;

class A
{
public:
   A(int& thing) : m_thing(thing) {}
   void printit() { cout << m_thing << endl; }

protected:
   const int& m_thing; //usually would be more complex object
};


int main(int argc, char* argv[])
{
   int myint = 5;
   A myA(myint);
   myA.printit();
   return 0;
}
```

Is there a name to describe this idiom? I am assuming it is to prevent the possibly large overhead of copying a big complex object?

Is this generally good practice? Are there any pitfalls to this approach?

**Answer**: 

> Is there a name to describe this idiom?

In UML it is called aggregation. It differs from composition in that the member object is not owned by the referring class. In C++ you can implement aggregation in two different ways, through references or pointers.

> I am assuming it is to prevent the possibly large overhead of copying a big complex object?

No, that would be a really bad reason to use this. The main reason for aggregation is that the contained object is not owned by the containing object and thus their lifetimes are not bound. In particular the referenced object lifetime must outlive the referring one. It might have been created much earlier and might live beyond the end of the lifetime of the container. Besides that, the state of the referenced object is not controlled by the class, but can change externally. If the reference is not const, then the class can change the state of an object that lives outside of it.

> Is this generally good practice? Are there any pitfalls to this approach?

It is a design tool. In some cases it will be a good idea, in some it won't. The most common pitfall is that the lifetime of the object holding the reference must never exceed the lifetime of the referenced object. If the enclosing object uses the reference after the referenced object was destroyed, you will have undefined behavior. In general it is better to prefer composition to aggregation, but if you need it, it is as good a tool as any other.

For const-correctness I'd write:

```c
using T = int;

class A
{
public:
  A(const T &thing) : m_thing(thing) {}
  // ...

private:
   const T &m_thing;
};
```
but a problem with this class is that it accepts references to temporary objects:

```c
T t;
A a1{t};    // this is ok, but...

A a2{T()};  // ... this is BAD.
```
It's better to add (requires C++11 at least):

```c
class A
{
public:
  A(const T &thing) : m_thing(thing) {}
  A(const T &&) = delete;  // prevents rvalue binding
  // ...

private:
  const T &m_thing;
};
```

### [Is it legal for class template specialisations to inherit from different base classes?](https://stackoverflow.com/questions/51504334/is-it-legal-for-class-template-specialisations-to-inherit-from-different-base-cl)

I have encountered a situation where my class template partial specialisations share a lot of code and it makes sense to move that into a base class. However it does not make sense for all of the specialisations to have the same base class.

```c
struct foo_base_1 { void bar() { std::cout << "base 1" << std::endl; }; };

struct foo_base_2 { void bar() { std::cout << "base 2" << std::endl; }; };

template <typename A, typename B>
struct foo { };

template <typename A>
struct foo<A, int> : foo_base_1 { };

template <typename A>
struct foo<A, double> : foo_base_2 { };

int main()
{
    foo<int, int> x;
    foo<int, double> y;

    x.bar();
    y.bar();
}
```
I realise that despite being specialisations of the same class they are effectively different types. Still, it feels wrong that the same class could inherit from different bases.

What I would like is some reassurance that this is OK.

**Answer:** 
Each template instantiation is a separate class of its own. Just as any ordinary class can inherit from whichever base classes you desire, template instantiations can do so as well. Consider the following example:

```c
template <typename Base>
class Derived : public Base
{
};
```
Absolutely legal...

This is, for instance, the base of the curiously **recurring template pattern**.

---

### 