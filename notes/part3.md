# Part 3 Design Principles Intro

* SOLID Principles are how you write clean mid-level code (module level)
* a module can be thought of basically as a source code file
* how do you arrange functions and data structures into classes, and how do you connect these classes
* goal of SOLID: create mid-level structures that 
	* tolerate change, 
	* are easy to understand, 
	* are the basis of generic components

* Single Responsibiliy Principle: a module has exactly one reason to change
* Open Closed: you change systems by adding new code, not changing existing code
* Liskov substitution principle: for a system to have interchangeable parts, those parts must adhere to a common interface so the can be substituted for one another.
* Interface segregation: don't depend on things you don't use
* Dependency inversion: High level policy code should not depend on low level detail code.

# Chapter 7: single responsibility principle

* the name implies that it means that a module should only do one thing - but that's not what it means - it is a principle that a _function_ that should only do one thing, but it's not the same as this principle.
* this is higher level: a module should have exactly one reason to change
* said another way: _a module should be responsible to one and only one actor_ - an actor being a group of users or stakeholders
* separate code that different actors depend on

## Symptoms of violation

* say an `Employee` class has methods `calculatePay`, `reportHours`, `save`
* `calculatePay` is for accounting, `reportHours` is for HR, `save` is for the database administrators
* these actors are now coupled together, meaning changes to one could impact another. eg if calc and report both depend on a `regularHours` function.
* different teams could end up working on the same source code, resulting in merge conflicts.

## solutions

* In general, move the functions into different classes
* you could separate the data stucture from the methods, with independent classes containing the methods for each actor or use case
* (you might then put a _facade_ on top which brings them back together - without impacting the independence)

# Chapter 8: the open closed principle

* a module should be open for extension but closed for modification
* a good architecture, when introducing a new behaviour, will require _zero_ change to existing code.
* this is achieved by separating things that change for different reasons (SRP) and properly organizing the dependencies between those things (the DIP)
* so separate your processes into classes, and your classes into components, and make sure the dependency arrow between them only point in one direction (and the direction they point is from concrete to abstract, low-level to high level)
* Since your detail, low-level stuff will change more frequently than your abstract, high level stuff, this also means the things that the components that change more often, when they are changed you don't have to re-write code in the high-level components.

* you will use a lot of interfaces to do this. Interfaces are used:
	* to make sure your dependencies flow the right way, from low to high level
	* to hide detail about high level components from the low level components that depend on them (thus providing them with some protection from change of the things they depend on)

# Chapter 9: Liskov Substitution

* in full: if for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behaviour of P is unchanged when o1 is substituted for o2, then S is a subtype of T

* the LSP guides the use of inheritence at the class level
* but it can and should be used at the module/component/architecture level

# Chapter 10: Interface Separation

* code shouldn't depend on other code it does't use, because then it could be affected when that code changes.
* At he module level it's harmful to depend on modules that contain more than you need

# Chapter 11: Dependency inversion

* dependencies refer to abstractions, not to concretions
* in principle, imports should only refer to modules conataining interfaces
* in practice, concrete dependencies can be relied upon when we know they will not change (e.g. builtins like String)
* interfaces are inherently less volatile that their concrete implementations, so code to them
* volatile concrete classes: don't refer to them, don't derive from them, don't override their functions, never mention their name
* nonetheless, concrete objects need to be instantiated. this is what factories are for.
* you should be able to draw a line on your UML diagram which separates the concrete from the abstract, with all dependencies pointing the same way = and architectural boundary
* The flow of control will flow in the _opposite_ direction, from the abstract to the concrete. This is why it's called the dependency inversion principle.
* DIP violations can not be fully eliminated - but they can be grouped in a single concrete component. This is generally `main`, which will do all your concrete stuff
