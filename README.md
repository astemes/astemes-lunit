# LUnit
[![Image](https://www.vipm.io/package/astemes_lib_lunit/badge.svg?metric=installs)](https://www.vipm.io/package/astemes_lib_lunit/) 
[![Image](https://www.vipm.io/package/astemes_lib_lunit/badge.svg?metric=stars)](https://www.vipm.io/package/astemes_lib_lunit/)
[![Test Runner](https://github.com/astemes/astemes-lunit/actions/workflows/workflow.yml/badge.svg)](https://github.com/astemes/astemes-lunit/actions/workflows/workflow.yml)

A LabVIEW Unit Testing Framework built on the [xUnit](https://en.wikipedia.org/wiki/XUnit) unit testing industry standard and inspired by [JKI VI Tester](https://github.com/JKISoftware/JKI-VI-Tester).
LUnit helps you test drive your LabVIEW development and provides the following key features.

- Fast test execution\*
- Clear and informative results view
- Run specific tests quickly from right-click menu
- Test results visible directly in LabVIEW project
- Instant loading of UI
- Parallel test execution managed by framework
- Dynamic test methods for testing of class hierarchies
- Profiling tools for test execution time and code coverage
- Native CLI and support for CI
- LabVIEW API

Full documentation available on [https://lunit.astemes.com](https://lunit.astemes.com/).

\*Test execution speed is really best-in-class. 
Below is a comparison between LUnit and two other popular unit testing toolkits\*\*.
The benchmark is available [here](https://github.com/Astemes/astemes-lunit/tree/main/sandbox/Benchmark) and consists of 160 tests in 20 test case classes.
The times reported are averages over 5 runs started by the `Run All` feature of each framework.
As the tests themselves are empty, the benchmark measures only the overhead added by the framework.

<img src="https://raw.githubusercontent.com/Astemes/astemes-lunit/main/docs/10_Basics/img/Benchmark.png" alt="Benchmark"  width="400"/>

\*\* The numbers are results from two test environment and you will get different results with other setups.
The conclusion is that LUnit is somewhere in the range of 2x-10x faster than other available tools.
If you find significantly differing results, please get in touch.
As of version 1.6 of LUnit the performance was further improved by more than 2x when running tests in parallell, which is not shown in numbers above.

## Prerequisites

To use LUnit you will need to have LabVIEW 2020 32-bit, LabVIEW 2020 64-bit SP1, or any later version of LabVIEW installed.
Please note that for 64-bit LabVIEW, version LabVIEW 2020 SP1 or later is required to work as expected.
As LUnit was developed using the LabVIEW Community Edition there is currently no support for older versions of LabVIEW.
Operation system requirements are Windows 10 (version 1909)/8.1 Update 12/7 SP13, Windows Server 2016, Windows Server 2012 R22, or Windows Server 2008 R2 SP13.

## Installation

LUnit is easiest installed using [VI Package Manager](https://www.vipm.io/package/astemes_lib_lunit/)(VIPM).
New releases are published to VIPM regularly, but lags behind the latest stable version published at [https://github.com/astemes/astemes-lunit/releases/](https://github.com/astemes/astemes-lunit/releases/).
The release contains a VIPM package which is installed using VIPM.

## Using LUnit

Once installed, LUnit is integrated into the LabVIEW development environment and the functionality is accessed through the Tools->LUnit menu.
From the menu, you may create a new tests or open the LUnit UI to run tests.

When you have created a Test Case class, you can create new test VI:s using the provided template or by creating static or dynamic dispatch VI:s on the Test Case class.
Static dispatch is recommended, unless you are using the test inheritance feature, as they have less overhead and does not lock the Test Case classes when the UI is loaded.

All tests within a Test Case class may executed from the project explorer, by right-clicking on a Test Case and selecting `Run Test Case...` from the menu.
As of LUnit version 1.5, a test VI may be executed by simply running it in LabVIEW using the run arrow.
There is also a toolbar button added to the LabVIEW Project Explorer to run all tests in project or create a new test case.

## Documentation

Documentation is available at [https://lunit.astemes.com](https://lunit.astemes.com).
A 15 minute demonstration and introduction to LUnit, recorded in 2024, is available [here](https://www.youtube.com/watch?v=Cxb1FUIsC04).
A presentation introducing LUnit was given at GLA Summit 2021 and the video recording is available [here](https://www.youtube.com/watch?v=Kys_w2RNffw&t=131s).

## Examples

There are a few examples installed with LUnit and these may be found through the NI Example Finder using the keyword `LUnit`.
The examples are installed at `C:\Program Files (x86)\National Instruments\LabVIEW 20XX\examples\Astemes\LUnit` by default.

## Is it free and open source?

Yes, absolutely!
LUnit is released by [Astemes](https://www.astemes.com) under the MIT license.
If you find it useful, please consider starring the project on GitHub and VIPM.

## Versioning

LUnit uses semantic version in the format major.minor.fix.build. 
The build version is of little significance as it only denotes the number of the build.
The fix indicates a bug fix, minor feature, or other improvement. 
As new major features are released or the number of minor feature releases accumulates, the minor version is incremented.
Minor version updates are uploaded to VIPM and NI Tools Network after an initial testing period.
These updates should be non-breaking and should not require any changes to the client code.
A major version update would mean that code developed using an earlier version might need modification.
There are currently no plans to make any major updates.

## Support

LUnit is an open source toolkit provided as is and without guarantees by [Astemes](https://www.astemes.com). If you encounter issues, use GitHub Issues to report and track the progress. If you have a suggestion for a solution, please consider making pull request. For paid-for professional support, please [contact Astemes directly](https://www.astemes.com/contact).

## Contribute

If you find LUnit useful, please share it with your colleagues and network to help grow the user base.
Also, consider starring the project on VIPM or GitHub to let us know that you like it.
If you find a bug, use the Issues section on GitHub.

To take a more active role, please feel free to fork the project and make a pull request. See `CONTRIBUTING.md` for more info on the development stack / process.
