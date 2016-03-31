# Script for a talk

## Introduction

- introduce yourself
- highlight what the talk is about

## Preaching

### Polls

- Who really loves testing?
- Who practices Test-Driven Development (TDD)?
- Who writes Property-based tests?

### Goal of this talk

At the end of this talk, hopefully, you will be able to:

- write less tests,
- with less maintenance burden,
- while covering more functionality;
- find all the hardest bugs in your systems;
- think harder and deeper about your problem domain.

### Very short definition on what PBT is

It is a style of testing, in which programmer writes generic assertions about properties that a SUT should fulfill.

On contrary to EBT, programmer does not specify concrete values for a data, rather only kinds of data (generators).

Simple example:

For all **integer** `x` and **integer** `y`, assert that: `x + y == y + x`.

### Plan for the talk

1. Language-agnostic guide about Property-Based Testing (PBT).
2. How does PBT and TDD play together?
3. Examples of PBT in different programming languages.
4. Writing your own PBT framework: what might you need for that?
5. Bottom line.

## Language-Agnostic Guide about PBT

### Start from Example-Based Testing (EBT)

- good coverage -> a lot of EBT code
- a lot of EBT code -> huge maintenance burden:
  * too much coupling too implementation details
  * especially if you do Mockist-style Unit testing
- EBT don't deliver intent well, they simply specify inputs and expected outcomes,
  * not **why** these inputs lead to these expected outcomes

Solving an unrelated failing EBTest might involve:
- some test failed, in part of the code, that is not related to your change,
- you don't understand (and nobody on the team does) what this failure is about,
- you decide to copy current real outcome and update the test's assertion,
- ...
- yeah.

### PBT as a very powerful compliment for typical Unit testing

- instead of figuring out all edge cases and providing examples for them,
- you simply generate them;
- it still makes sense to have a one or two example tests to serve as a usage documentation;
- effectively you do much more testing with much less code;
- this means lower maintenance is required,
- though harder and more intense, and involved maintenance :),
- which is a good thing,
- since you don't end up fixing failing tests by blindly updating assertions;
- additionally it makes you think harder about:
  * the problem at hand,
  * the business domain,
  * the code,
  * (sometimes for hours, and, oh my god, talking to other team members and stakeholders, maybe even users);
- PBT allows you to be less specific about the outcome,
- and say more about what is actually important about outcome.

### Humans are good at interpolation, Computers aren't

- Show some nice graph here with people's assumptions and interpolations,
- and some real data, that is in bizarre manner outside of interpolations.

### 100 tests in one

- probably 100 times harder to write, though :)

### Generators and their importance to the understanding of business domain

- Limit usage of built-in basic-type generators and write your own custom generators for your domain types/objects/data structures. This allows for much deeper understanding of the problem at hand. Also allows to have much smarter shrinking strategies.
- Talk about shrinking a bit, and why it is cool (some examples of non-shrinked output and shrinked output).

- Coming up with a generator may be even hard sometimes, and, oh my god, involve talking to people on the team, to business people, experts in the domain, stakeholders, and even users! I personally consider it a good thing.

- They can be actually used in EBT too (or in REPL), and very helpful at that;
- they are like test factories, only much more powerful.

- Generators are very good documentation for your code,
- since they specify, what is the valid input and output of your functions/methods,
- and what is the valid form/structure, that your domain objects can have.

- Building generators from small to large (by composition).

- Building generators is an investment
- And return on the investment includes:
  * better understanding of the domain for yourself and future readers of your code,
  * they can be re-used in other properties,
  * they can be re-used in other generators,
  * they can be re-used in conventional EBT,
  * they can be used anywhere, where you need to construct a domain object/data structure without being specific about some parts of it, e.g.: in REPL to try something out.

### Labeling and classification for property checks' results

- Labels allow to provide a better failing message
- Classification allow to do exploratory testing for the nature of your problem and your implementation

### True meaning of "Reasoning about the code"

- I can reason about the code well, when:
  * I don't care what exactly happens under the hood,
  * Rather, I can say with confidence what is true about outcome for the given input
  * "X => Y"

(input and outcome here include initial state of the world and side effects,
not only input and output arguments).

- EBT is about concrete pairs of input => outcome
- PBT is about generic pairs of (Any Input of Form X) => (Outcome of Form Y)
  * i.e.: "X => Y"

- EBT tells you which inputs produce which outcomes,
- but it does not tell you **WHY** these inputs produce these outcomes,
- PBT does tell you exactly **WHY**.

### PBT naturally will drive you to the Ports & Adapters architecture

- You can't just run 100 tests per each single piece of knowledge in your system
- with the database backing the whole thing,
- so you will have to use Ports & Adapters architecture,
- that allows you to use different adapter for the same interface,
- which could be a `Fake`, `In-Memory`, `File-Based`, etc.

- Such `Fake` implementations doesn't have to always be fast and always return success. Actually this is pretty useful to instruct them to be slow, return timeouts and failures in specific properties to see how your code handles that.
- Additionally, this will actually make you handle these cases. I rarely see code, that uses database directly having any error handling if database goes away, or just too slow.
- This way PBT makes you think more about unhappy paths, as opposed to thinking only about happy paths in typical EBT.
- It makes it painful to ignore corner cases.

- Not only it will encourage better architecture, as a bonus, it makes following Liskov Substitution Principle (L part of SOLID principles) really simple:
  * per definition: for each property P(x), that holds for a type T, it should hold for its subtype S;
  * this means, that all property-based tests for supertype have to hold for subtypes, otherwise LSP is violated;
  * this enables huge test suite code re-use, when subtyping or inheritance is involved + protection from violating LSP = two birds with one stone.

### PBT as a monitoring tool in production

- You can really re-use properties to run checks on your real production data continuously.
- This way PBT are much more useful then EBT,
- since what is important in PBT is important in production.

### How to write properties properly?

- Common mistake: spelling out the implementation or part of the implementation in the property.
- Instead, one should use one of these concepts:
  * Symmetry
  * Multiple paths
  * Induction,
  * Idempotence,
  * Invariants,
  * Consistency,
  * "Hard to solve, easy to verify",
  * Test Oracles,
  * State Machine Model (for stateful System Under Test).

### Impact of PBT

- Less code, less maintenance and more coverage!;
- You think much more about types/objects in your domain (even if your language is very dynamically typed);
- Allows to find bugs not only in System Under Test, but to find inconsistencies in the specification itself (in test code and domain understanding);
- Allows to test more, than can be formally verified, e.g.: normal distribution of some combinatorial data;
- Allows for exploration of your data and behavior by using classification features of PBT. This way you can understand how fast and on which input your system under test is, and how much trivial and non-trivial data you have produced by your generators;
- Leads to better design decisions.

## How PBT and TDD play together?

- In EBT, at times, it is hard to make a small incremental change to make the current failing test pass, and programmer simply have to implement the whole solution to make it pass. It is no longer test-driven solution and may contain un-tested bits.
- On contrary in PBT that is not the case. Properties verify cross-cutting concepts of the system under test and one by one drive incremental implementation. Though, it is quite important to remember the rule of TDD:
  * "As tests get more specific, code gets more generic".
  * But this is applicable only to EBT..
  * If we take the most useful essence of this statement:
  * "As tests get more constraining, code gets more generic".
  * I can apply this to properties now:
  * "As properties get more constraining, code gets more generic".
  * So to do TDD with PBT it is important to start with less constraining (more generic) properties and go towards more constraining (less generic) properties, while the system under tests gets more generic, until it meets desired requirements.
  * Example of less constraining property is that area of a figure under test is non-negative, instead of verifying some formula right away.
  * Starting from less constraining properties, additionally, provides a safety net in case you get some latter property or generator wrong. They will highlight a problem immediately. And getting less constraining properties right is simpler.

- In EBT, often, "tests recycling" takes place, where you remove some tests, that were created only to bootstrap your solution at first stages.
- Not exactly the same, but similar happens in PBT:
  * you might have to remove some properties, when they are implied by other properties.

- Additional benefit of PBT, is that properties may find bugs not only in the system under the test, and also in other properties. This is not the case with EBT: if you have a bug in test A it will not be revealed by test B.

- I found it interesting that doing TDD with PBT forces your to first test your generators, once you want to use them to test your SUT. This involves figuring out, what actually is a valid data and what is not for your SUT. The code used to test generators can be immediately re-used for validation of the input. That is another nice side-effect of using TDD with PBT.

## Examples of PBT in different programming languages

### Haskell example

Library of choice is original `QuickCheck`.

#### Add

#### Mul

#### Sort

### Clojure example

Library of choice is `org.clojure/test.check`.

#### Add

#### Mul

#### Sort

### Ruby example

Library of choice is gem `rantly`, homepage: https://github.com/abargnesi/rantly.

#### Add

#### Mul

#### Sort

### Golang example

Library of choice is `testing/quick` from standard library. I was surprised
Golang has such a library in the standard library.

#### Add

#### Mul

#### Sort

### Crystal example

Library of choice is shard https://github.com/waterlink/quick.cr

#### Add

#### Mul

#### Sort

### Example generators

(only Crystal here)

#### UppercaseLetter generator for Diamond Kata

#### GameGen for Bowling Kata

## Building your own PBT library

### First of all, it is not necessary to use library at all for this

A simple loop and bunch of calls to random will do.
They will get repetitive, and if you are doing PBT seriously you need to have
proper generators. You still can have generators by conforming to some interface,
like `.next() : T` method or something. Then you need shrinking, otherwise failures
are not that readable and helpful. Once you do that on your own, you will probably
end up with your own implementation of PBT, that hopefully can be extracted to
the library.

### If you set out to build such a library with nice APIs for a programming language

that doesn't have it? Or if it is not good enough in your opinion?

What do you think you need?

1. Cover all basic types with built-in generators, so that users don't have to
   implement them over and over again.
2. Come up with nice interface for a generator, so that users can implement
   their own generators easily.
3. Allow for easy combination of generators, for example let your users express:
   `an Array of UppercaseLetters` or `HashMap of UppercaseLetter to Integer`, etc.
4. Build actual property checking engine, that will allow your users to specify
   data they need (by providing generators, built-in or custom, or combinations)
   and property they want to check (this is probably some lambda expression, or
   function/method/object if you don't have lambdas in your programming language).
5. Property checking engine should ask generators for data and supply this data
   to the property and see if property returns success (you define what success is:
   boolean, exception/no-exception or value of specific type). It should do it
   N times (usually 100). Oh, and you should give your user chance to control N.
6. Last necessary part of PBT is shrinking. Without good shrinking mechanisms in
   place, failures produced by random data are not that useful at all. So provide
   sane shrinking mechanism. Define default shrinking strategies on built-in
   generators. And give your user simple way to define their own strategies for
   their custom generators.

Optional parts:

- labeling to allow user to improve readability of failures
- classification to allow user to do exploration on quantifiable properties of generators and SUT

## Bottom line

Write less tests. Generate them.
