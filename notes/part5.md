# Part 5: Architecture

## 15: What is architecture

* Architecture is the shape given to a system by those who build it
* it takes the form of division into components, their arrangement, and how they communicate
* the purpose of an architecture is to support the lifecycle of the system, making it easy to develop, deploy, operate and maintain.
* the strategy is to leave as many options open as possible for as long as possible
* a small team developing a small system doesn't benefit from much of an architecture, and too much of one can be a burden. 
* a good architecture should make a system deployable with a single action.
* the impact of architecture on operation is less dramatic, because you can solve it with more hardware. And hardware is cheaper than people. That said, a good architecture reveals the operation of a system, meaning it is more easily understandable.
* Maintainence is costly, and a good architecture minimises those costs by making it easy to debug, repair and add new features, and by minimising the risk of breaking the system by doing this.

### Keeping options open

* the value in a system is in both its behaviour and structure. THe second has more value - it's what makes software _soft_, flexible.
* systems are composed of policy and details. Policy is business rules and procedures, this is where the value is. Details are the things necessary to enact policy; IO, DB, web, servers etc.
* a good architecture makes detail irrelevant to policy and so allows decisions about detail to be delayed.
* it also means you can experiment with detail. Don't like how persistance is handled? You can swap it out with something else without having to change policy. Experimentation gives you information, which means when you do 'decide' on a detail, you have all the information you need.

## 16: Independence

### Use cases, Operation, development and deployment

* in the last chapter we said that an architecture should reveal the intent of the system - in other words it should support the use-cases of it's purpose. 
* But architecture doesn't have much influence over behaviour of a system, it can only reveal. When a developer using your shopping cart app is looing for a behaviour, they shouldn't have to look hard, because all the behaviours will be 1st class elements, very visible and descriptively named.
* if the system must handle 100,000 requests a second, the architecture must support that. Through multithreading, distributed microservices, multiple processes etc. but this is something a good architecture will leave open; through proper isolation of components 
* from a development standpoint, the architecture must support independent developability by many teams, so they don't interfer with eachother
* for deployment, a good architecture means you can 'deploy now', without lots of configurations scripts and fiddling with property files
* this is hard because we don't _know_ all of these requirements when we start building, and they will change over time anyway
* the way we solve for this is leaving options open for as long as possible

### Decoupling layers and use cases
* You need your system to support all current and future use cases, but you don't know what they are. What you _do_ know is the basic intent of the system.
* knowing that, you can use the SRP and CCP to group things that change for the same reason in the context of the system. That implies UI are grouped, but separate from business rules. 
* some business rules are specific to the application, some are general to the entire business. the latter will usually change at a slower rate. keep them separate too.
* the database, obviously, should be separate
* you can think of use cases as vertical slices through the system, with elements of UI, business logic, database etc. If you are decoupling in both horizontal and vertical dimensons, you are very unlikely to have to change exiting code to add new features

### Decoupling mode
* when you get down to operations, having these elements separated means you can give each element the resources it needs to achieve it's required throughput.
* But only if they have the appropriate mode; i.e. they must be independent services.

### Independent Developability and Deployability

### Duplication

## 17: 

## 18: 

## 19: 

## 20: 

## 21: 

## 22: 

## 23: 

## 24: 

## 25: 

## 26: 

## 27: 

## 28: 

## 29: 

