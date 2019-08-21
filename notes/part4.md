# Part 4: Component principles

* SOLID is how to make bricks into walls, CPs are about how to make walls into buildings.
* what components are, what they are made of, how they are composed into sysytems

## Chapter 12: Components

* units of deployment, the smallest thing that can be deployed. jars, gems, dlls, binaries.
* that is _independently_ deployable. and so independently _developable_

## Chapter 13: Component Cohesion

* what classes belong in what components?
* three principles: Reuse/Release equivalent (REP), common closure (CCP), common reuse (CRP)

### Reuse/Release Equivalence

* classes and modules that are formed into a component must belong to a cohesive group, connected by some theme or purpose
* this is a 'weak principle' since it doesn't give you concrete rules, but combined with the other two it comes into action

### Common Closure

* gather things that have a common change pattern, separate things that have a different change pattern (a common change pattern is when things change for the same reasons and at the same times)
* Basically the SRP for components - a _component_ shouldn't have multiple reasons to change
* maintainability is more important than reusability - you would rather a change occur in one component than many
* if two classes are so tightly bound together that a change in one necessitates a change in the other, they belong in the same component.

### Common Reuse
* Don't force users of a component to depend on things they don't need
* If things tend to be reused together they belong in the same component. If not they don't
* when we depend on a component, we want to make sure we depend on every class in that component. That every class in a component is inseparable. If not, why put it in the component?
* Like ISP: don't depend on things you don't need

### tensions between component principles

* notice REP and CCP are inclusive, they want you to group things when they have a common purpose or common change pattern. CRP is exclusive, it wants you to exclude things that don't absolutely need to be there.
* if you focus on inclusion you will have too many releases, if you split it up too much your code becomes harder to reuse. 
* a good architect finds a position in the triangle which bests meets the current needs of the dev team.

## Chapter 14: component coupling

* three principles on relationships between components

### Acyclic dependencies principle

* allow no cycles in the component dependency graph
* the "morning after syndrome" - you come in and your code doesn't work because someone stayed later than you and changed something you depend on
* a 'periodic integration' is not scalable, you will spend more time integrating than coding
* the answer is to partition the environment into releasable components, with a component being a unit of work, with a single dev being responsible. when it works, they release it to other devs, with a release number, and other devs can adopt or not.
* BUT this only works if there are no _cycles_. If you can draw a component diagram in which you can never follow the arrows and end up back where you started, you have a _directed acyclic graph_.
* As soon as you have a cycle between components, in effect they become one big component, they are no longer independently releasable. 
* to break a cycle:
	* apply the DIP, put an interface in the middle
	* or create a new component in the middle which has both arrows coming into it - a component sized interface

### Implications for upfront design of component structure

* the component structure can't be designed from the top down, it evolves as the system changes
* it might seem intuitive that components will represent the functionality of the system - but this isn't the case.
* they are a map to the buildability and maintainability of the application - so you can't design them upfront
* as the software evolves, the architect will mold the dependency graph to protect stable elements (business rules) from volitile ones (GUI) - i.e. the CCP
* as it gets even bigger, the CRP will start to influence - you want to maximize reuse
* then, as cycles appear, the ADP comes into play

### Stable Dependencies Principle

* depend in the direction of stability
* Even when you design software to be easy to change, it can be made difficult to change by someone else who makes themselves dependent on it.
* following the SDP ensures modules that are intended to be easy to change are not depended on by modules that are harder to change
* when we say stability, we mean it is hard to change. 
* A component with a lot of incoming dependencies is hard to change because it will take a lot of work to reconcile all of those changes.
* if something has a module which depends on it, that something is responsible to that module
* if something depends on nothing, it is independent - it will have no external pressure on it to change
* if something is independent and responsible it is stable, as in not likely
* if something is dependent and irresponsible it is unstable, since it will have external pressure to change, and no internal resistance.

* a measure of stability: I = OD / (OD + ID), where I = instability index, OD = outgoing dependencies, ID = incoming dependcies
* OD and ID are AKA fan-out and fan-in
* an I=0, i.e. 0 outgoing dependencies, is maxmimally stable. 1 is maximally instable.
* the SDP says that I of a component should be larger than the I of the components it depends on - I should decrease in the direction of dependency

* when you have a violation of the SDP, create a component which contains an interface which inverts the dependency. This component will be super stable (it will have OD=0) 


### Stable Abstractions Principle

* If you want a component to be stable, make it abstract.

#### The SAP defined and measuring abstraction in components
* if high level policies (business and architectural decisions) are put into stable components it makes them hard to change. So how do we maintain flexibility in our high level policies?
* OCP says you should create classes that are open to extention but not modification. Abstract classes conform to this principle (??)
* SAP says stable components should be abstract so they can be extended, and unstable components should be concrete, since their instability means they are easy to change
* combining the SDP and SAP gives you the DIP: If you depend in the direction of stability, and stability is abstraction, then your dependencies should be on abstractions.
* As applied to components, the DIP is a little more grey: a class is either abstract or it isn't, the level of abstraction of a component is more difficult to pin down
* A metric for the level of abstraction of a component is its ratio of abstract to total classes, a value referred to as _A_, ranging from 0 (very concrete) to 1 (entirely abstract).
* SAP says _A_ and _I_ should be inversely proportional, that is on a scatter plot of the two metrics the dots should follow the main sequence. 
* Things that are both very abstract (high A) and very unstable (High I) are in the _zone of uselessness_ (since they are never used) and very concrete
* very stable components (near 0,0) are in the _zone of pain_ since they will be very rigid but very difficult to change. 
* A database schema is a notorious zone of pain, since it's both stable and concrete. note that this is only the case because when it is volatile: you want to change it but it is hard to change. Compare that to a builtin String class. It is also around (0,0), but since it will rarely change is is non-volatile, and therefore pretty harmless in the 0,0 zone
* The rule, therefore, is to keep volatile components away from the zone of pain, and on the main sequence.
* The distance from the main sequence is _D = |A+I-1|_. Consider restructuring things with a high D value

