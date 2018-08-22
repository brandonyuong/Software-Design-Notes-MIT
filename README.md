# Software-Design-Notes-MIT

Goals of software construction (Java):
1)  Safe from bugs:  Correct today and correct in the unknown future
2)  Easy to understand:  Communicating clearly with future programmers, including future you
3)  Ready for change:  Designed to accommodate change without rewriting


WEEK 1:  Static Checking

Static checking can verify type consistency. It doesn't check values and couldn't detect an overflow.

Static checking tends to be about types, errors that are independent of the specific value that a variable has. A type is a set of values. Static typing guarantees that a variable will have some value from that set, but we don't know until runtime exactly which value it has. So if the error would be caused only by certain values, like divide-by-zero or index-out-of-range then the compiler won't raise a static error about it.

Dynamic checking, by contrast, tends to be about errors caused by specific values.

Summary

The main idea we introduced today is static checking. Here's how this idea relates to the goals of the course:

    Safe from bugs. Static checking helps with safety by catching type errors and other bugs before runtime.

    Easy to understand. It helps with understanding, because types are explicitly stated in the code.

    Ready for change. Static checking makes it easier to change your code by identifying other places that need to change in tandem. For example, when you change the name or type of a variable, the compiler immediately displays errors at all the places where that variable is used, reminding you to update them as well.


WEEK 2:  Code Review

Principles of good code: safe from bugs, easy to understand, and ready for change.

In every code review, look for principles of good code regardless of programming language or program purpose.

1.  Don't Repeat Yourself (DRY)
        Duplicated code is a safety risk; makes harder to clean bugs.  
        Avoid copy-pasting.
		
2.  Comments where needed
        Use comments judiciously to promote good coding.  
        Comment assumptions and references.  
        Avoid comments that transliterate code to English.
		
3.  Fail fast
        Code should reveal bugs ASAP.  
        Static checking fails faster than dynamic checking, which fails faster than no checking.  
        Avoid quietly passing wrong answers.
		
4.  Avoid magic numbers
        0 and 1 are usually given in CS.  All other numbers are magic.
        Use constants, arrays.  Use names.
        Bad numbers are computed by hand by the programmer.  Not clear to the reader.
		
5.  One purpose for each variable
        Don't reuse parameters and variables.  Issues are less safe from bugs and less ready for change.
	
6.  Use good names
        Best names are long and self-descriptive.  Consequently needs less comments.
        `tmp`, `temp`, and `data` are awful names.
        Abbreviations are unclear.
		
7.  No global variables
        Don't use anything that is both variable and global (public static without final).
        "public" means accessible anywhere, "static" means not associated with an object.
        Change global variables into parameters and return values.
        Or, put them inside objects that you're calling methods on.

8.  Return results, don't print them
        Returning results allow them to be reused in another context.
        Only highest level parts of a program should interact with a human user or console.
        Low level parts should take their input as parameters and return the output as results.

9.  Use whitespace for readability
        Use consistent indentation.
        Use spaces within code lines to make them easy to read.
        Never use tab characters, only space characters.  (tab character != Tab key).
        Set programming editor to insert space characters with the Tab key.

Three key properties of good software as follows:

    Safe from bugs. In general, code review uses human reviewers to find bugs. DRY code lets you fix a bug in only one place, without fear that it has propagated elsewhere. Commenting your assumptions clearly makes it less likely that another programmer will introduce a bug. The Fail Fast principle detects bugs as early as possible. Avoiding global variables makes it easier to localize bugs related to variable values, since non-global variables can be changed in only limited places in the code.

    Easy to understand. Code review is really the only way to find obscure or confusing code, because other people are reading it and trying to understand it. Using judicious comments, avoiding magic numbers, keeping one purpose for each variable, using good names, and using whitespace well can all improve the understandability of code.

    Ready for change. Code review helps here when it's done by experienced software developers who can anticipate what might change and suggest ways to guard against it. DRY code is more ready for change, because a change only needs to be made in one place. Returning results instead of printing them makes it easier to adapt the code to a new purpose.


WEEK 3:  Testing Software

Testing process, validation, includes formal reasoning (verification), code review, and testing.

Testing difficult: exhaustive testing infeasible (too many cases to cover), haphazard testing only works when program is more likely to fail than succeed (buggy program not a good design), and random testing not effective for software (discontinuous & discrete behavior, abrupt failure, unlike physical artifacts that fail slowly)

Goal of tester is to *make software fail*, beating it where it is vulnerable.  
Test-first programming = write specification for function, test the specification, write actual code

Good test suite = *partition* into a set of cases/subdomains that is small enough to run quickly, yet large enough to validate the program.
E.g. at -1, 0, 1, small numbers, large numbers, positive, negative.  At *boundaries* (0, max and min of numeric types, emptiness, first and last element of a collection)

Black box testing:  external perspective - choosing test cases only from specification without looking at code.
White box testing:  internal perspective - requires programming skills.  Aka glass box testing.  Choose test cases with knowledge of how the function is implemented.

Coverage:
    1. Statement coverage: is every statement run by some test case?  (100% statement coverage is common goal, but rarely achieved)
    2. Branch coverage: for every if or while statement in the program, are both the true and the false direction taken by some test case?  (100% is highly desirable, but has more arduous criteria)
    3. Path coverage: is every possible combination of branches — every path through the program — taken by some test case?  (Infeasible, requires exponential-size test suites to achieve)

Unit testing: testing an individual module in isolation
Stubs:  a dummy, mock version of a module that is called by a module being tested

Testing should be automated, no inputs or interaction.  A good testing framework, like JUnit, helps build automated test suites.
    You still have to come up with good test cases.  Automatic test generation is a hard problem & subject of active CS research.
Regression testing - running all tests after every change.
    Regression test:  a test case that checks for previously fixed bugs.

Summary

In this reading, we saw these ideas:

    Test-first programming. Write tests before you write code.
    Partitioning and boundaries for choosing test cases systematically.
    White box testing and statement coverage for filling out a test suite.
    Unit-testing each module in isolation as much as possible.
    Automated regression testing to keep bugs from coming back.

The topics of today's reading connect to our three key properties of good software as follows:

    Safe from bugs. Testing is about finding bugs in your code and test-first programming is about finding them as early as possible, immediately after you introduced them.

    Easy to understand. Testing doesn't help with this as much as code review does.

    Ready for change. Readiness for change was considered by writing tests that only depend on behavior in the spec. We also talked about automated regression testing, which helps keep bugs from coming back when changes are made to code.


WEEK 4:  Specifications

    Understand preconditions and postconditions in method specifications, and be able to write correct specifications;
    Be able to write tests against a specification;
    Know the difference between checked and unchecked exceptions in Java; and
    Understand how to use exceptions for special results

Specifications describe how a program should behave.  Should be written down to help during debugging.  It's a firewall between the client and the implementer.  It allows teamwork and development of each unit of code without requiring full knowledge of the system.

In this reading, we will cover:  preconditions, postconditions, exceptions.

The precondition is an obligation on the client (i.e., the caller of the method). The postcondition is an obligation on the implementer of the method. If the precondition holds for the invoking state, the method is obliged to obey the postcondition, by returning appropriate values, throwing specified exceptions, modifying or not modifying objects, and so on.

The overall structure is a logical implication: if the precondition holds when the method is called, then the postcondition must hold when the method completes.

If the precondition does not hold when the method is called, the implementation is not bound by the postcondition. It is free to do anything, including not terminating, throwing an exception, returning arbitrary results, making arbitrary modifications, etc.

Javadoc: example documentation comment:
/**
 * Find a value in an array.
 * @param arr array to search, requires that val occurs exactly once
 *            in arr
 * @param val value to search for
 * @return index i such that arr[i] = val
 */

Null references:
In Java, references to objects and arrays can also take on the special value null, which means that the reference doesn't point to an object. Null values are an unfortunate hole in Java's type system.

Primitives cannot be null, but we can assign null to any non-primitive variable.
The compiler happily accepts this code at compile time. But you'll get errors at runtime because you can't call any methods or use any fields with one of these references:  you'll get a NullPointerException.

In good Java programming — null values are implicitly disallowed in parameters and return values.

Avoid null. It is unpleasantly ambiguous. It's rarely obvious what a null return value is supposed to mean — for example, Map.get(key) can return null either because the value in the map is null, or the value is not in the map. Null can mean failure, can mean success, can mean almost anything. Using something other than null makes your meaning clear.

Specifications of methods should talk about parameters and return values of the methods, rather than local variables or private fields.  Source code is often unavailable to the reader of the spec.

Black box tests: chosen only with the specification in mind.
Glass box tests: chosen with knowledge of the actual implementation (Testing).  Even glass box tests must follow the specs.

Checked exceptions are checked by the compiler.  (e.g. using "throws exception")
Unchecked exceptions are used to signal bugs.  Not expected to be handled by code except perhaps at the top level.

Summary

A specification acts as a crucial firewall between the implementor of a procedure and its client. It makes separate development possible: the client is free to write code that uses the procedure without seeing its source code, and the implementor is free to write the code that implements the procedure without knowing how it will be used.

Let's review how specifications help with the main goals of this course:

    Safe from bugs. A good specification clearly documents the mutual assumptions that a client and implementor are relying on. Bugs often come from disagreements at the interfaces and the presence of a specification reduces that. Using machine-checked language features in your spec, like static typing and exceptions rather than just a human-readable comment, can further reduce bugs.

    Easy to understand. A short, simple spec is easier to understand than the implementation itself, and saves other people from having to read the code.

    Ready for change. Specs establish contracts between different parts of your code, allowing those parts to change independently as long as they continue to satisfy the requirements of the contract.


WEEK 5:  Designing Specifications

    Understand underdetermined specs and be able to identify and assess nondeterminism
    Understand declarative vs. operational specs and be able to write declarative specs
    Understand strength of preconditions, postconditions, and specs; and be able to compare spec strength
    Be able to write coherent, useful specifications of appropriate strength


Deterministic specification:  when presented with a state satisfying the precondition, the outcome is completely determined

    Nondeterministic: behaves randomly even if called with the same inputs.  I.e. code depends on a random number.  Specification which is not deterministic doesn't have to have nondeterministic implementation.  Can be satisfied by deterministic implementation.

Underdetermined:  specifications which are not deterministic.  Can be resolved by several deterministic implementations, to be decided by the implementer at implementation time.  An underdetermined spec is typically implemented by a fully deterministic implementation.

Operational specifications give a series of steps that the method performs; pseudocode descriptions are operational.

Declarative specifications don't give details of intermediate steps. Instead, they just give properties of the final outcome and how it's related to the initial state.  Almost always, declarative specifications are preferable.

One reason programmers sometimes lapse into operational specifications is because they're using the spec comment to explain the implementation for a maintainer. Don't do that. When it's necessary, use comments within the body of the method, not in the spec comment.

A specification S2 is stronger than or equal to a specification S1 if

    S2's precondition is weaker than or equal to S1's,
    and
    S2's postcondition is stronger than or equal to S1's for the states that satisfy S1's precondition.

If this is the case, then an implementation that satisfies S2 can be used to satisfy S1 as well, and it's safe to replace S1 with S2 in your program.

These two rules embody several ideas. They tell you that you can always weaken the precondition; placing fewer demands on a client will never upset them. And you can always strengthen the post-condition, which means making more promises.


Good Spec Guidelines:

The specification should be coherent:
    The spec shouldn't have lots of different cases. Long argument lists, deeply nested if-statements, and boolean flags are all signs of trouble. Incoherent specs could indicate that a method is trying to do several things... a method should do one thing.

The results of a call should be informative.  No ambiguity.

The spec should be strong enough.  Spec should give clients a strong enough guarantee in the general case.  Careful of special cases that undermine an otherwise useful method.

The spec should also be weak enough.  What if bad input values?
    E.g. effects: opens a file named filename
    Spec should say something much weaker: that it attempts to open a file, and if it succeeds, the file has certain properties.

The spec should use abstract types where possible.

Use preconditions or not?  The most common use of preconditions is to demand a property precisely because it would be hard or expensive for the method to check it.  The decision of whether to use a precondition is an engineering judgment. The key factors are the cost of the check (in writing and executing code), and the scope of the method. If it's only called locally in a class, the precondition can be discharged by carefully checking all the sites that call the method. But if the method is public, and used by other developers, it would be less wise to use a precondition. Instead, like the Java API classes, you should throw an exception.


Access Control:

Keeping internal things private makes your class's public interface smaller and more coherent (meaning that it does one thing and does it well). Your code will be easier to understand.

We will see even stronger reasons to use private in the next few classes, when we start to write classes with persistent internal state. Protecting this state will help keep the program safe from bugs.

Summary

A specification acts as a crucial firewall between implementor and client — both between people (or the same person at different times) and between code. As we saw in the previous reading, it makes separate development possible: the client is free to write code that uses a module without seeing its source code, and the implementor is free to write the implementation code without knowing how it will be used.

Declarative specifications are the most useful in practice. Preconditions (which weaken the specification) make life harder for the client, but applied judiciously they are a vital tool in the software designer's repertoire, allowing the implementor to make necessary assumptions.

As always, our goal is to design specifications that make our software:

    Safe from bugs. Without specifications, even the tiniest change to any part of our program could be the tipped domino that knocks the whole thing over. Well-structured, coherent specifications minimize misunderstandings and maximize our ability to write correct code with the help of static checking, careful reasoning, testing, and code review.

    Easy to understand. A well-written declarative specification means the client doesn't have to read or understand the code. You've probably never read the code for, say, Python dict.update, and doing so isn't nearly as useful to the Python programmer as reading the declarative spec.

    Ready for change. An appropriately weak specification gives freedom to the implementor, and an appropriately strong specification gives freedom to the client. We can even change the specs themselves, without having to revisit every place they're used, as long as we're only strengthening them: weakening preconditions and strengthening postconditions.


WEEK 6: Avoiding Debugging

First Defense - Make Bugs Impossible
Make them impossible by design:  static checking.  Dynamic checking.  Immutability (type, references).

Second Defense - Localize Bugs.
Fail fast with assertions.  Checking preconditions is an example of defensive programming. 

Note that the Java assert statement is a different mechanism than the JUnit methods assertTrue(), assertEquals(), etc. They all assert a predicate about your code, but are designed for use in different contexts. The assert statement should be used in implementation code for defensive checks inside the implementation. JUnit assert...() methods should be used in JUnit tests to check the result of a test. The assert statements don't run without -ea, but the JUnit assert...() methods always run.


What to Not Assert:
1.  Assert bugs in your code.  Avoid trivial assertions, like you would avoid uninformative comments.  No need to find bugs in the compiler or Java virtual machine, components which you should trust unless you have good reasons to doubt them.

2.  External failures (existence of files, availability of a network, or correctness of human input) are not bugs and there is no change you can make to your program in advance that will prevent them from happening. External failures should be handled using exceptions instead.

3.  Since assertions may be disabled, the correctness of your program should never depend on whether or not the assertion expressions are executed. In particular, asserted expressions should not have side-effects.


Incremental Development:
1.  Unit testing:  testing a module in isolation.
Regression testing:  testing bugs that have been found and fixed before

2.  Modularity:  dividing a system into components or modules, each of which can be tested and reused separately from the rest of the system.  The opposite of a modular system is a monolithic system — big and with all of its pieces tangled up and dependent on each other.

3.  Encapsulation:  building walls around a module so that it is responsible for its own internal behavior.  One type of encapsulation is access control, using "public" and "private" to control visibility and access of variables and methods.  Keeping things private as much as possible, especially for variables, provides encapsulation, since it limits the code that could inadvertently cause bugs.

Another kind of encapsulation comes from variable scope. The scope of a variable is the portion of the program text over which that variable is defined.


Minimizing Variable Scope:
1.  Always declare a loop variable in the for-loop initializer.
2.  Declare a variable only when you first need it and in the innermost curly-brace block that you can.
3.  Avoid global variables.

Summary

In this reading, we looked at some ways to minimize the cost of debugging.

Avoid debugging. Make bugs impossible with techniques like:

    static typing
    automatic dynamic checking
    immutable types and references

Keep bugs confined. Make the search for buggy code easier with techniques like:

    failing fast
    incremental development and unit testing
    scope minimization

Thinking about our three main measures of code quality:

    Safe from bugs. We're trying to prevent them and get rid of them.
    Easy to understand. Techniques like static typing, final declarations, and assertions are additional documentation of the assumptions in your code. Variable scope minimization makes it easier for a reader to understand how the variable is used because there's less code to look at.
    Ready for change. Assertions and static typing document the assumptions in an automatically-checkable way so that when a future programmer changes the code, accidental violations of those assumptions are detected.


WEEK 7:  Mutability

String is an example of an immutable type. A String object always represents the same string. StringBuilder is an example of a mutable type. It has methods to delete parts of the string, insert or replace characters, etc.

Getting good performance is one reason why we use mutable objects. Another is convenient sharing: two parts of your program can communicate more conveniently by sharing a common mutable data structure.


Risk of Mutation:

Why choose immutable type of the mutable type is more powerful?
The answer is that immutable types are safer from bugs, easier to understand, and more ready for change. Mutability makes it harder to understand what your program is doing and much harder to enforce contracts.

Mutable types may increase performance and reduce repeat code, but passing around mutable objects could result in a bug that is hard to track down.

Is it ready for change? Obviously the mutation of the date object is a change, but that's not the kind of change we're talking about when we say “ready for change.” Instead, the question is whether the code of the program can be easily changed without rewriting a lot of it or introducing bugs.

Mutable objects can be bad for performance.  To avoid bugs, we would have to copy defensively.  But defensive copying forces the program to do extra work and use extra space for every client.  We end up with lots of copies.  If we used an immutable type instead, then different parts of the program could safely share the same values in memory, so less copying and less memory required.  Immutable types never need to be defensively copied, so they are more efficient than mutable types.

Having multiple references, or aliases, for the same mutable object is what causes mutable types to be risky.  They are safe if used locally and with only one reference to the object.


Specifications for mutating methods:

When a method performs a mutation, it is crucial to include that mutation in the method's spec.

If the *effects* (postcondition) of a spec do not explicitly say that an input can be mutated, then in 6.005 we assume mutation of the input is implicitly disallowed. Virtually all programmers would assume the same thing. Surprise mutations lead to terrible bugs.

Mutable objects can make simple contracts very complex:
This is a fundamental issue with mutable data structures. Multiple references to the same mutable object (also called aliases for the object) may mean that multiple places in your program — possibly widely separated — are relying on that object to remain consistent.

Mutable objects reduce changeability:
Mutable objects make the contract between a client and an implementer more complicated and reduce the freedom of the client and implementer to change. In other words, using objects that are allowed to change makes the code harder to change. Here's an example to illustrate the point.

Summary

In this reading, we saw that mutability is useful for performance and convenience, but it also creates risks of bugs by requiring the code that uses the objects to be well-behaved on a global level, greatly complicating the reasoning and testing we have to do to be confident in its correctness.

Make sure you understand the difference between an immutable object (like a String) and an immutable reference (like a final variable). Snapshot diagrams can help with this understanding. Objects are values, represented by circles in a snapshot diagram and an immutable one has a double border indicating that it never changes its value. A reference is a pointer to an object, represented by an arrow in the snapshot diagram and an immutable reference is an arrow with a double line, indicating that the arrow can't be moved to point to a different object.

The key design principle here is immutability: using immutable objects and immutable references as much as possible. Let's review how immutability helps with the main goals of this course:

    Safe from bugs. Immutable objects aren't susceptible to bugs caused by aliasing. Immutable references always point to the same object.

    Easy to understand. Because an immutable object or reference always means the same thing, it's simpler for a reader of the code to reason about — they don't have to trace through all the code to find all the places where the object or reference might be changed because it can't be changed.

    Ready for change. If an object or reference can't be changed at runtime, then code that depends on that object or reference won't have to be revised when the program changes.


WEEK 8: Debugging

Reproduce the Bug

Start by finding a small, repeatable test case that produces the failure. If the bug was found by regression testing, then you're in luck; you already have a failing test case in your test suite. If the bug was reported by a user, it may take some effort to reproduce the bug. For graphical user interfaces and multithreaded programs, a bug may be hard to reproduce consistently if it depends on timing of events or thread execution.

Understand the Location and Cause of the Bug

To localize the bug and its cause, you can use the scientific method:

    Study the data. Look at the test input that causes the bug and the incorrect results, failed assertions, and stack traces that result from it.

    Hypothesize. Propose a hypothesis, consistent with all the data, about where the bug might be or where it cannot be. It's good to make this hypothesis general at first.

    Experiment. Devise an experiment that tests your hypothesis. It's good to make the experiment an observation at first — a probe that collects information but disturbs the system as little as possible.

    Repeat. Add the data you collected from your experiment to what you knew before and make a fresh hypothesis. Hopefully you have ruled out some possibilities and narrowed the set of possible locations and reasons for the bug.

Other tips:

    Bug localization by binary search.

    Prioritize your hypotheses. When making your hypothesis, you may want to keep in mind that different parts of the system have different likelihoods of failure.

    Swap components. If you have another implementation of a module that satisfies the same interface, and you suspect the module, then one experiment you can do is to try swapping in the alternative.

    Make sure your source code and object code are up to date. Pull the latest version from the repository.

    Get help. It often helps to explain your problem to someone else.

    Sleep on it. If you're too tired, you won't be an effective debugger. Trade latency for efficiency.

Fix the Bug

    Once you've found the bug and understand its cause, the third step is to devise a fix for it. Avoid the temptation to slap a patch on it and move on. Ask yourself whether the bug was a coding error, like a misspelled variable or interchanged method parameters, or a design error, like an underspecified or insufficient interface. Design errors may suggest that you step back and revisit your design, or at the very least consider all the other clients of the failing interface to see if they suffer from the bug too.

    Think also whether the bug has any relatives. If I just found a divide-by-zero error here, did I do that anywhere else in the code? Try to make the code safe from future bugs like this. Also consider what effects your fix will have. Will it break any other code?

    Finally, after you have applied your fix, add the bug's test case to your regression test suite and run all the tests to assure yourself that (a) the bug is fixed, and (b) no new bugs have been introduced.

Summary

In this reading, we looked at how to debug systematically:

    reproduce the bug as a test case and put it in your regression suite
    find the bug using the scientific method
    fix the bug thoughtfully, not slapdash

Thinking about our three main measures of code quality, systematic debugging is essentially about safety from bugs: we're trying to get rid of a bug, while using regression testing to keep it from coming back.


WEEK 9:  Abstract Data Types

Abstraction Meaning:

    Abstraction. Omitting or hiding low-level details with a simpler, higher-level idea.

    Modularity. Dividing a system into components or modules, each of which can be designed, implemented, tested, reasoned about, and reused separately from the rest of the system.

    Encapsulation. Building walls around a module (a hard shell or capsule) so that the module is responsible for its own internal behavior, and bugs in other parts of the system can't damage its integrity.

    Information hiding. Hiding details of a module's implementation from the rest of the system, so that those details can be changed later without changing the rest of the system.

    Separation of concerns. Making a feature (or “concern”) the responsibility of a single module, rather than spreading it across multiple modules.


Classifying Types and Operations:

Types, whether built-in or user-defined, can be classified as mutable or immutable. 

The operations of an abstract type are classified as follows:

    Creators create new objects of the type. A creator may take an object as an argument, but not an object of the type being constructed.  E.g. constructors, factory methods, constants.

    Producers create new objects from old objects of the type. E.g. artihmetic operators, string concatenation, String.substring(), String.toUpperCase().  No mutations involved.

    Observers take objects of the abstract type and return objects of a different type. The size method of List, for example, returns an int.  Other examples: comparison operators, accessors (get), length(), min(), max().

    Mutators change objects. E.g. add(), remove(), set(), addAll(), sort().


    Schematic distinctions between each classification:
    creator: t* → T
    producer: T+, t* → T
    observer: T+, t* → t
    mutator: T+, t* → void | t | T

    These show informally the shape of the signatures of operations in the various classes. Each T is the abstract type itself; each t is some other type. The + marker indicates that the type may occur one or more times in that part of the signature, and the * marker indicates that it occurs zero or more times. | indicates or.


Designing an Abstract Type:

Designing an abstract data type involves choosing good operations and determining how they should behave. Here are a few rules of thumb.

It's better to have a few, simple operations that can be combined in powerful ways, rather than lots of complex operations.

Each operation should have a well-defined purpose and should have a coherent behavior rather than a panoply of special cases.

The set of operations should be adequate in the sense that there must be enough to do the kinds of computations clients are likely to want to do. A good test is to check that every property of an object of the type can be extracted.

The type should not mix generic and domain-specific features.


Representation Independence:

Critically, a good abstract data type (ADT) should be representation independent. This means that the use of an abstract type is independent of its representation (the actual data structure or data fields used to implement it), so that changes in representation have no effect on code outside the abstract type itself. For example, the operations offered by List are independent of whether the list is represented as a linked list or as an array.

You won't be able to change the representation of an ADT at all unless its operations are fully specified with preconditions and postconditions, so that clients know what to depend on, and you know what you can safely change.

ADT's existing clients depend only on the specs of its public methods, not on its private fields, we can make this change without having to inspect and change all that client code. That's the power of representation independence.


Testing an Abstract Data Type:

We build a test suite for an abstract data type by creating tests for each of its operations. These tests inevitably interact with each other. The only way to test creators, producers, and mutators is by calling observers on the objects that result, and likewise, the only way to test observers is by creating objects for them to observe.

Summary

    Abstract data types are characterized by their operations.
    Operations can be classified into creators, producers, observers, and mutators.
    An ADT's specification is its set of operations and their specs.
    A good ADT is simple, coherent, adequate, and representation-independent.
    An ADT is tested by generating tests for each of its operations, but using the creators, producers, mutators, and observers together in the same tests.

These ideas connect to our three key properties of good software as follows:

    Safe from bugs. A good ADT offers a well-defined contract for a data type, so that clients know what to expect from the data type, and implementors have well-defined freedom to vary.

    Easy to understand. A good ADT hides its implementation behind a set of simple operations, so that programmers using the ADT only need to understand the operations, not the details of the implementation.

    Ready for change. Representation independence allows the implementation of an abstract data type to change without requiring changes from its clients.


WEEK 10:  Abstraction Functions and Rep Invariants

Today's reading introduces several ideas:

    invariants
    representation exposure
    abstraction functions
    representation invariants

In this reading, we study a more formal mathematical idea of what it means for a class to implement an ADT, via the notions of abstraction functions and rep invariants. These mathematical notions are eminently practical in software design. The abstraction function will give us a way to cleanly define the equality operation on an abstract data type (which we'll discuss in more depth in a future class). The rep invariant will make it easier to catch bugs caused by a corrupted data structure.


Invariants:

A good ADT preserves its own invariants.  An invariant is a property of a program that is always true, for every possible runtime state of the program.  The ADT is responsible for ensuring its own invariants hold and doesn't depend on good behavior from clients.

Representation exposure:  code outside the class can modify the representation directly.
Avoid this by using immutable types, defensively copying mutable types, hide direct references to mutable types, and implementing immutable wrappers around mutable types.


Rep Invariant and Abstraction Function:

Space of rep values (R):  values of the actual implementation entities.
Space of abstract values (A):  values that the type is designed to support.

Key qualities:
    1.  Every abstract value is mapped to by some rep value.
    2.  Some abstract values are mapped to by more than one rep value.
    3.  Not all rep values are mapped.

In practice, we can only illustrate a few elements of the two spaces and their relationships; the graph as a whole is infinite. So we describe it by giving two things:

1. An abstraction function that maps rep values to the abstract values they represent:

    AF : R → A

In the terminology of functions, the properties we discussed above can be expressed by saying that the function is surjective (also called onto), not necessarily injective (also called one-to-one), and often partial.

2. A rep invariant that maps rep values to booleans:

    RI : R → boolean

For a rep value r, RI(r) is true if and only if r is mapped by AF. In other words, RI tells us whether a given rep value is well-formed. Alternatively, you can think of RI as a set: it's the subset of rep values on which AF is defined.

The essential point is that designing an abstract type means not only choosing the two spaces — the abstract value space for the specification and the rep value space for the implementation — but also deciding what rep values to use and how to interpret them.

It's critically important to write down these assumptions in your code, as we've done above, so that future programmers (and your future self) are aware of what the representation actually means. Why? What happens if different implementers disagree about the meaning of the rep?

TA Discussion Comments:

    RI and AF concern the internal representation of the object. They are only for the implementor and therefore, not part of the specifications.

    The key idea of data abstraction is that a type is characterized by the operations you can perform on it. 


Documenting the AF, RI, and Safety from Rep Exposure:

It's good practice to document the abstraction function and rep invariant in the class, using comments right where the private fields of the rep are declared. We've been doing that above.

Another important piece of documentation is a rep exposure safety argument. This is a comment that examines each part of the rep, looks at the code that handles that part of the rep (particularly with respect to parameters and return values from clients, because that is where rep exposure occurs), and presents a reason why the code doesn't expose the rep.

Here's an example of Tweet with its rep invariant, abstraction function, and safety from rep exposure fully documented:

// Immutable type representing a tweet.
public class Tweet {

    private final String author;
    private final String text;
    private final Date timestamp;

    // Rep invariant:
    //   author is a Twitter username (a nonempty string of letters, digits, underscores)
    //   text.length <= 140
    // Abstraction Function:
    //   represents a tweet posted by author, with content text, at time timestamp 
    // Safety from rep exposure:
    //   All fields are private;
    //   author and text are Strings, so are guaranteed immutable;
    //   timestamp is a mutable Date, so Tweet() constructor and getTimestamp() 
    //        make defensive copies to avoid sharing the rep's Date object with clients.

    // Operations (specs and method bodies omitted to save space)
    public Tweet(String author, String text, Date timestamp) { ... }
    public String getAuthor() { ... }
    public String getText() { ... }
    public Date getTimestamp() { ... }
}


Establishing Invariants:

An invariant is a property that is true for the entire program — which in the case of an invariant about an object, reduces to the entire lifetime of the object.

This means the invariant is true of all instances of the abstract data type:

    1.  creators and producers must establish the invariant for new object instances
    2.  mutators and observers must preserve the invariant
    3.  no representation exposure occurs,


ADT invariants replace preconditions

Now let's bring a lot of pieces together. An enormous advantage of a well-designed abstract data type is that it encapsulates and enforces properties that we would otherwise have to stipulate in a precondition. For example, instead of a spec like this, with an elaborate precondition:

/** 
 * @param set1 is a sorted set of characters with no repeats
 * @param set2 is likewise
 * @return characters that appear in one set but not the other,
 *  in sorted order with no repeats 
 */
static String exclusiveOr(String set1, String set2);

We can instead use an ADT that captures the desired property:

/** @return characters that appear in one set but not the other */
static SortedSet<Character> exclusiveOr(SortedSet<Character>  set1, SortedSet<Character> set2);

This is easier to understand, because the name of the ADT conveys all the programmer needs to know. It's also safer from bugs, because Java static checking comes into play, and the required condition (sorted with no repeats) can be enforced in exactly one place, the SortedSet type.

Summary

    An invariant is a property that is always true of an ADT object instance for the lifetime of the object.
    A good ADT preserves its own invariants. Invariants must be established by creators and producers and preserved by observers and mutators.
    The rep invariant specifies legal values of the representation and should be checked at runtime with checkRep().
    The abstraction function maps a concrete representation to the abstract value it represents.
    Representation exposure threatens both representation independence and invariant preservation.

The topics of today's reading connect to our three properties of good software as follows:

    Safe from bugs. A good ADT preserves its own invariants, so that those invariants are less vulnerable to bugs in the ADT's clients, and violations of the invariants can be more easily isolated within the implementation of the ADT itself. Stating the rep invariant explicitly, and checking it at runtime with checkRep(), catches misunderstandings and bugs earlier, rather than continuing on with a corrupt data structure.

    Easy to understand. Rep invariants and abstraction functions explicate the meaning of a data type's representation and how it relates to its abstraction.

    Ready for change. Abstract data types separate the abstraction from the concrete representation, which makes it possible to change the representation without having to change client code.
