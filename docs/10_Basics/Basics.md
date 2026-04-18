# LUnit Basics

This document walks through the basic workflow using LUnit to test LabVIEW code.
If you prefer to watch a video, there is an introduction available on [this link](https://www.youtube.com/watch?v=Cxb1FUIsC04).

## Prerequisites

To follow along with the instructions on this page you will need to have LabVIEW version 2020 or later installed as well as the LUnit unit testing framework.

## Creating a Test Case

All tests you write will belong to a test case class.
This is implemented as a LabVIEW class, but in order to use it you will not need to know anything about object oriented programming.

To get started, create an empty project and add a test case class to it by clicking the New Test Case button in the toolbar of the LabVIEW project.

![Project integration](img/project_Integration.png)

You can also do the same from the ``Tools > LUnit > New Test Case...`` menu option.

![Tools Menu > Lunit > new test case](img/tools_menu_new_tc.jpg)

Save the test case in a convenient location.
Some like to keep the tests next to the code they are testing, and others keep them in a separate folder called ``Tests`` or similar.
I personally find the latter option with a separate top level directory the most convenient.
Keep in mind that tests should not be included in builds and there should be no dependencies pointing from your code to the test code.

## Adding a Test Method

Now you have a test case and may add some test methods to the test case.
A test method is a VI belonging to the test case class and will get executed by the framework.

If you are using LUnit version 1.x, the name of the VI **must** start with the four letters "test" (case insensitive).
In LUnit 2.0, this requirement has been relaxed and any public vi in a test case class is considered a test and enumerated in the UI.
If the vi contains assertions, it will report results when the test is run.
Helper VIs should be made `private` within the test case class, which will avoid them being shown and run by the UI.
It is **not** recommended to make test VIs *dynamic dispatch*.
To create a new test method, right-click on the _Test Method Template.vit_ and select ``New from Template``.

![New from template](img/new_static_from_template.png)

You can create test methods any way you like and you are free to delete the template method.
It is important however that the connector pane uses the same pattern of terminals as the template.
The error in terminal is optional and never used when running tests by the framework.

You should now make your test method test something useful by implementing the block diagram of the vi.
To perform tests you will use the assertions available in the provided palette, or via Quick Drop.

![Simple test case](img/simple_test_case.png)

## Using Assertions

The result of each test is determined using assertions.
There is a set of assertions to choose from, as shown in the figure above, and the names should be self explanatory.
One test method may contain multiple assertions and the result from each assertion will show up in the result view.

One pro-tip is that the ``Pass if Equal.vi`` assertion also works well for array data types, clusters and data classes.
The result of comparing arrays will show up in the result view as shown below.

![Comparing Arrays](img/array_comparison.jpg)

Please note that the ``Pass if Equal.vi`` assertion will fail if either the type or the value does not match. 

## Running the Test Case

You can run a single test VI (using the Run Arrow) and it will run and show the user interface with the results of the test.
When run this way, the vi is actually run twice. 
First once, which causes the UI to launch and then a second time which is the actual test execution.
In this way, the UI can run the `Setup.vi` dynamic dispatch, the test method and then finally the `Teardown.vi` method even when the test is not started from the UI.

Starting from version 1.10 of LUnit, a Quick Drop plugin is available which allows running tests using a Quick Drop shortcut.
From Quick Drop press ``Ctrl + L`` to run all tests within the current project.
If you open Quick Drop from a VI and press ``Ctrl + Shift + L`` LUnit will run all tests from Test Case classes calling this specific VI. 
This can be very useful for checking if an edit to a VI caused any test to fail.
Please note that this feature requires all tests to be loaded into memory, *i.e.* included in the active project, and it can only check for static links.
The last limitation means that the feature will not work for dynamic dispatch VIs, as their call sites are not statically known.

To run all tests contained in a test case, you can right click it in the project window and select the ``Run Test Case...`` menu option.

![Run from right click menu](img/run_test_case.png)

This will open the test execution user interface and run the test case.
Alternatively you can also launch the user interface from the tools menu through the ``Tools > LUnit > LUnit UI...`` menu option.
This will open the user interface and show all tests in the current project.
As the test is run, the results are also shown as visual icon overlays in the project explorer.

![Run from right click menu](img/test_execution_ui.png)

## Adding utility VIs

It is common that code is reused between tests belonging to a test case class, and could then be placed in subVIs or so-called test utility VIs.
If you create such VIs belonging to the class, make sure to restrict the access scope (*i.e.* make the VIs ``private`` or ``protected``), as the UI enumerates all public VIs in the class.

## Using the Setup and Teardown methods

You can add a Setup and a Teardown method to the test case by overriding the corresponding dynamic dispatch VIs.
The Setup VI will run once before each test method in the test case and the Teardown will run once after the test method is completed.
This is useful in some cases, but should not be overused as it makes the test methods less self-contained.
If you need to pass data from the Setup VI to the test method VI or Teardown VI, you can bundle the data into the test case class wire.

![Run from right click menu](img/setup_test_teardown.png)

## Organizing Tests

It does make sense to organize tests in some manner, especially as the number of tests increase.
There are many common practices around where to keep tests and how to organize the folder structure on disk. 
In general it is useful to keep tests separated from the source, as there should be no dependency from the source on the test code.

While developing, it is convenient to keep the tests within the active LabVIEW project, as they may then be run quickly through the [project integration](#running-the-test-case) and you can run all the tests in the project through the tools menu option.
As test suites grow, it becomes less convenient as the test time accumulates with larger test suites and some of the tests might not be relevant to the feature under development.
While it is important to run the whole suite to catch regression issues, this does not have to be done as frequently as tests covering the new feature.

A good workflow is to keep a separate LabVIEW project file as a test suite where all tests are collected.
Tests may be moved from the active project into this test suite and the test suite may then be added to the active LabVIEW project as a project item. 
All tests within the test suite project may then be executed using the right click menu option, but will not be executed when running all tests in current project from the tools menu option and will not need to be loaded with the project.

![Run nested project](img/run_nested_project.png)

When executing tests in a Continuous Integration environment, this test suite LabVIEW project file is a good entry point for running tests.

In LUnit version 1.3.1, an additional feature was introduced to help speeding up test execution by skipping slow or unrelated tests.
A test method (`vi`) or entire test case class (`lvclass`) may be marked with an underscore `_` character as the first character, making it into a `dashed` test class or test case.
`Dashed` tests may be skipped when running through the user interface by using the menu option `Ignore Dashed Tests` as shown below.

![Dashed tests](img/dashed_tests.jpg)

Notice that the test "_test Broken.vi" and the entire "_test Failing Test Case.lvclass" were not executed during the test run in the previous image.
By toggling the `Ignore Dashed Tests` menu option, it is straight forward to optimize test execution for either execution speed or test coverage.
In a typical test driven development workflow, test execution speed must be very fast as the tests are executed repeatedly on time scale of minutes.
It is however important to catch regressions by regularly running the full test suite, including the dashed tests.
When running LUnit using the API, as would be the case on a Continuous Integration server, dashed tests are never skipped.