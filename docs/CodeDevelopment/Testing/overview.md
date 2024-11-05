# Tests

Testing firmware is critical and should be done so much more than it is today. So please, start writing tests. A lot
of work has been done to make it easier.

## Static Code Tests (analysis)

*stuart_ci_build* provides a framework for running static tests on the code base.  More details of the ever changing
tests can be found here. <https://github.com/microsoft/mu_basecore/tree/release/202002/.pytool/Plugin>

## UEFI Unit Tests

Host based unit tests allow you to run your tests on the same machine in which you are compiling your code.  These tests will run
as applications within the operating system host environment. This is the preferred route when possible as these
tests will automatically roll into the CI process and are much faster and easier to run. Obviously this means you
will need to write your code and tests with limited UEFI dependencies.  Any dependency your code has will need to be
mocked or faked for the unit test scenario.

There are two frameworks available in Tianocore and Project Mu basecore.

Implementation details here. <https://github.com/microsoft/mu_basecore/tree/release/202405/UnitTestFrameworkPkg>

### GoogleTest

The latest unit test framework supported by the UnitTestFrameworkPkg is GoogleTest and can be used to implement host-based unit tests. GoogleTest on GitHub is included in the UnitTestFrameworkPkg as a submodule. Use of GoogleTest for target-based unit tests of EDK II components is not supported. Host-based unit tests that require mocked interfaces can use the mocking infrastructure included with GoogleTest called gMock. Note that the gMock framework does not directly support mocking of free (C style) functions, so the FunctionMockLib (containing a set of macros that wrap gMock's MOCK_METHOD macro) was created within the UnitTestFrameworkPkg to enable this support. The details and usage of these macros in the FunctionMockLib are described later.

GoogleTest requires less overhead to register test suites and test cases compared to the Framework. There are also a number of tools that layer on top of GoogleTest that improve developer productivity. One example is the VS Code extension C++ TestMate that may be used to implement, run, and debug unit tests implemented using GoogleTest.

### Framework

The first supported framework is called Framework and is implemented as a set of EDK II libraries. The Framework supports both host-based unit tests and target-based unit tests that share the same source style, macros, and APIs. In some scenarios, the same unit test case sources can be built for both host-based unit test execution and target-based unit test execution. Host-based unit tests that require mocked interfaces can use the mocking infrastructure provided by cmocka that is included in the UnitTestFrameworkPkg as a submodule.


### Target Based (UEFI Firmware on a device)

Some testing just doesn't make sense to run as a host test.  Tests that rely on system hardware and system state might
only work as target tests.  The unit test Framework supports this and the implementation works for both.  This can
also include features that require reboots and saving state before the reboot so that tests can resume upon
continued execution.

## UEFI Shell Based Functional Tests

These can also leverage the UEFI target based tests.

## UEFI Shell Based Audit Tests

These tests are often one off UEFI shell applications that collect system data and then compare that data against known
good values for a system.  This is because these types of tests have no right or wrong answer.  Often we have a python
script/component to the test to compare expected result to actual result.  If the actual result doesn't match then this
type of test should fail.

## Testing Automation for physical hardware

The Project Mu team has done a lot of work with the open source project "robot framework".  This framework provides a
great logging and execution environment but at this time it is out of scope for Project Mu docs.  If you want to know
more, contact us as we are definitely willing to partner/share/engage.

## Testing Python

* Create pytest and/or python unit-test compatible tests.
* Make sure the python code passes the `flake8` "linter"
