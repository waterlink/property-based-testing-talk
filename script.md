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

- They can be actually used in EBT too (or in REPL), and very helpful at that;
- they are like test factories, only much more powerful.

- Generators are very good documentation for your code,
- since they specify, what is the valid input and output of your functions/methods,
- and what is the valid form/structure, that your domain objects can have.

- Building generators from small to large (by composition).

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

### PBT as a monitoring tool in production

- You can really re-use properties to run checks on your real production data continuously.
- This way PBT are much more useful then EBT,
- since what is important in PBT is important in production.

## How PBT and TDD play together?
