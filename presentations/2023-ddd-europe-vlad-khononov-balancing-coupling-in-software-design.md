Link: https://www.youtube.com/watch?v=KTy4rqgPOjg&list=WL

---

> _"System design is inherently about boundaries (what's in, what's out, what spans, what moves between), and about
tradeoffs. It reshapes what is outside, just as it shapes what is inside."_
>
> Ruth Malan

> _"There are many useful and revealing heuristics for defining the boundaries of a service. Size is one of the least
> useful."_
>
> Nick Tune

## What is coupling?

Coupling == Connected

You cannot build a system of **independent components**.

Components need to work together to achieve overarching goal. The value will be higher if components are working
together than working separately. So we cannot build system without coupling.

Coupling defines the components' degree of freedom. It sounds scary. But we do not want system with **unlimited degree
of freedom**.

## Dimensions of coupling

### Strength

#### Book "Composite/Structured Design" - types of coupling

From strongest to weakest

##### Content Coupling

Using implementation details to communicate (using information that was never intended for
integration). E.g. "GO TO" instruction, reflection

##### Common coupling

Communicating using global accessible shared memory space, which is unstructured byte array. Duplication of knowledge
how the data in that shared memory is structured. E.g. two services sharing bucket on S3 (it is unstructured by
default).

##### External coupling

The same as [Common Coupling](#common-coupling), but now there is structure. But it is primitive value or array of them.
E.g. two services talking through database.

##### Control coupling

Services are not encapsulating business logic properly. So the first service is telling second service how to do its
job. It is about functionality and pout encapsulation of it.

##### Stamp coupling

Communicating through data structures. Data structures are the modules' implementation details. If we are asking for
email we receive a lot of information with this email address.

##### Data coupling

Sharing minimum information. I am asking for email and I receive only email address. Minimum knowledge that is needed
for components to work together.

#### Book "Structured Design" - connascence (Latin: 'having been born together')

Connascence:

- change in one component requires change in another component
- postulate reasonable change that would require both components to change

Types of connascence:

- Static (compile time - reading code) - from the lowest amount of sharing knowledge
    1. Name - knowing only name of variable that you are about to read
    2. Type - knowing also type of variable that you are about to read
    3. Meaning - assign meaning to values like `status = 7` (both components need to know what does it mean)
    4. Algorithm - agree of usage of specific algorithm, e.g. encoding
    5. Position - order of passed values - method with 4 arguments of type String
- Dynamic (runtime)
    1. Execution - some operations must be executed in a specific order (like a receipt)
    2. Timing - Time-based dependency, e.g. timeout in Execution case
    3. Value - Values need to change in one atomic transaction, e.g. business invariant or setting edges of triangle
    4. Identity - Two objects must reference and work with the same instance of another object, e.g. Unit of Work

**Connascence and Coupling** are reflecting different aspects of manifestations of coupling.

#### Integration strength

From highest to lowest;

1. Intrusive Coupling

- integration though private interface
- fragile and implicit
- upstream could be unaware of this coupling

2. Functional Coupling

- functionally related (business knowledge)
- cascading changes
- don't have to be physically integrated (HTTP requests or database)

3. Model Coupling

- sharing a model of the business domain

4. Contract Coupling

- explicit integration contract (without implementation details)
- Open Host, ACL, Published Language

### Distance

Level of abstractions. If higher on list than lower response time, invest we need to invest to implement a change:

- Statements
- Methods
- Classes
- Namespaces/packages
- Components
- (Micro)services
- Systems

Cost of change is proportional to the distance between components.

If closer these affected components are to each other the higher probability of such a collateral cascading changes.

If we have one team vs two teams that need to change codebase in two microservices then we have a higher coordination
overhead for cross-team change.

### Volatility

_How often do the coupled components change?_
_How often do the upstream change?_

We can use subdomains to evaluate the expected rate of changes.

- Core - high volatility (a lot of changes) - should be upstream
- Supporting - low volatility
- Generic - low volatility - should be upstream too, because it is not changing a lot

## Combinations of dimensions

- High Strength + High Volatility = High Maintenance Effort
- High Strength + High Distance = High Coordination Effort (expensive change)
- High Distance + High Volatility = High Coordination Effort - evolve system

**Pain = Strength * Volatility * Distance**

The best option to be one of them **ZERO**.

Takeaways:

- Eliminate accidental coupling
- Reduce integration strength as much as possible
- If strength and volatility is high - reduce distance
