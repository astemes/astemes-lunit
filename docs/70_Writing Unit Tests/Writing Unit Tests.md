# Writing Unit Tests

This section is meant to give some advice to start writing unit tests and is not specific to LUnit in any way.
There is already a lot that has been written on the topic and there are a lot of good resources in the literature outside the LabVIEW sphere.
As the topic is not a science, this section should be viewed as the views and opinions of the author and not as facts.

## Why bother writing unit tests?

### Prove that the code works as expected

Unit tests provide a structured way to verify that code behaves as intended.
By writing explicit assertions about expected behavior, you create a formal specification of what the code should do.
This is especially valuable for edge cases and boundary conditions that might otherwise be overlooked during manual testing.

### Regression testing

As code evolves, changes in one area can inadvertently break behavior elsewhere.
A comprehensive test suite acts as a safety net, catching regressions automatically.
When a bug is found and fixed, adding a test for that specific case ensures the same bug cannot be reintroduced undetected.
Only fix the same bug once.

### Drive good design

Code that is difficult to test is often a signal that it is tightly coupled or has too many responsibilities.
The discipline of writing tests encourages thinking about interfaces, separation of concerns, and modularity.
This design pressure tends to produce code that is easier to understand and maintain.
Focusing on one aspect at a time when writing tests helps keep units of code small and focused.

### Confidence to refactor

Refactoring, *i.e.* changing the structureof code without changing its' behavior, is essential for keeping a codebase healthy over time.
A good test suite makes it possible to refactor with confidence, since any unintended behavioral change will immediately appear as a failing test.

### Tests as documentation

Tests describe how code is expected to be used and what results to expect.
A well-named test method with clear assertions can communicate the intent of code more directly than comments or external documentation.
Unlike written documentation, tests stay accurate as long as they pass.

## How to write unit tests

### Cohesive

Each test method should verify a single behavior or scenario.
A test that checks multiple unrelated behaviors is harder to diagnose when it fails and harder to maintain as the code evolves.

### Expressive

Test names should clearly describe what is being tested and what outcome is expected.
A reader should be able to understand what a test verifies without reading its implementation.
A name like `test Controls DO when Apply Voltage.vi` conveys more than `test case 3.vi`.

### Informative on failure

When a test fails, it should be immediately clear what went wrong.
Choose assertions that produce meaningful failure messages and structure tests so that the point of failure identifies the broken behavior.

### Number of assertions

Tests with fewer assertions are generally easier to diagnose since a failure points directly to one broken behavior.
That said, verifying a group of closely related properties in a single test is sometimes more readable than splitting them across many tests.

### Grouping of tests

Related tests belong in the same Test Case class.
A useful approach is to group tests by the unit under test, where a unit can be *e.g.* a VI or a class. 
This also makes it easier to locate tests when a specific part of the code changes.

### Independent

Tests should not depend on each other or on a particular execution order.
Each test should set up the conditions it needs and leave no state behind that could affect other tests.
Independence makes failures easier to trace and allows the test suite to be run in parallel.

## How not to write tests

### Interdependent tests

When tests rely on state left by a previous test, a failure in one test can cause cascading failures that are difficult to trace back to their root cause.
Write tests so that each one can be run in isolation, in any order.

### Testing timing

Tests that verify timing-dependent behavior tend to be fragile and slow.
Exact timing is affected by system load, hardware, and environment, making these tests unpredictable across machines.
It is usually better to separate timing concerns from the logic under test and test the logic independently.

### Global resources

Tests that share global state, *e.g.* global variables, FGVs, files, or network connections can interfere with one another.
Shared state makes tests order-dependent and can lead to failures that are difficult to reproduce.
Where possible, resources should be created and released within the test itself.

### Very slow tests

Tests that take a long time to run tend to be run less frequently, reducing their value.
Fast feedback is one of the main benefits of automated testing, so keeping individual tests fast matters.
If a test is inherently slow because it accesses hardware or an external system, consider whether it is better suited to an integration test rather than a unit test.

### Breaking encapsulation

Tests should interact with code through its public interface, just as production code does.
Tests that access private implementation details become brittle and they may break when the implementation changes even if the observable behavior does not.

### Overuse of setup and teardown

The Setup and Teardown lifecycle methods are useful for reducing duplication, but overusing them can make individual tests harder to understand.
When a test depends heavily on state initialized in Setup, a reader must mentally combine both methods to follow the logic.
Prefer to keep each test self-contained where the overhead is acceptable, and use Setup and Teardown for genuinely shared initialization that would otherwise be repeated.

### Testing private methods

Private methods are implementation details and are not directly accessible from outside their class.
In most cases, private behavior is exercised indirectly through the public interface that calls it, and testing through the public interface is generally preferable.
If a private method contains complex logic that is difficult to exercise fully through the public interface, it may be worth considering whether that logic should be extracted into a separate class with its own public interface and tests.

## When to write unit tests

### Before writing production code

Writing tests before writing production code, an approach known as Test-Driven Development (TDD), uses tests to define the expected behavior first.
The production code is then written to make the tests pass.
This approach ensures that every piece of production code has corresponding tests from the start and tends to produce small, well-defined units.
Writing the test first often leads to better design decissions, since the code is forced to be written to be testable. 

### While writing production code

Many developers find it practical to alternate between writing tests and writing code.
A small piece of functionality is implemented, a test is added to verify it, and the cycle repeats.
It is important to see that the test fails if when the implementation changes, and this should be verified if the test is written after the code.

### After writing production code

Tests can also be written after the fact, for example when working with an existing codebase.
Retroactive tests are particularly valuable for documenting current behavior before a refactoring, and for covering bug fixes.
The main risk is that tests written after the code may unconsciously mirror the implementation rather than the intended specification.

## Testing in isolation

A unit test should test one unit of code in isolation, without depending on other units, external systems, or real hardware.
When the unit under test has dependencies, those dependencies can be replaced with controlled substitutes.
The two most common types of substitute are stubs and mocks, and the general term for either is a *test double*.
To learn more about these topics, please see the LMock project. 
LMock provides automated test double generation. 
