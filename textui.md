The Command-Line Test Runner
============================

The PHPUnit command-line test runner can be invoked through the phpunit
command. The following code shows how to run tests with the PHPUnit
command-line test runner:

When invoked as shown above, the PHPUnit command-line test runner will
look for a ArrayTest.php sourcefile in the current working directory,
load it, and expect to find a `ArrayTest` test case class. It will then
execute the tests of that class.

For each test run, the PHPUnit command-line tool prints one character to
indicate progress:

`.`

> Printed when the test succeeds.

`F`

> Printed when an assertion fails while running the test method.

`E`

> Printed when an error occurs while running the test method.

`R`

> Printed when the test has been marked as risky (see risky-tests).

`S`

> Printed when the test has been skipped (see
> incomplete-and-skipped-tests).

`I`

> Printed when the test is marked as being incomplete or not yet
> implemented (see incomplete-and-skipped-tests).

PHPUnit distinguishes between *failures* and *errors*. A failure is a
violated PHPUnit assertion such as a failing `assertSame()` call. An
error is an unexpected exception or a PHP error. Sometimes this
distinction proves useful since errors tend to be easier to fix than
failures. If you have a big list of problems, it is best to tackle the
errors first and see if you have any failures left when they are all
fixed.

Command-Line Options
--------------------

Let's take a look at the command-line test runner's options in the
following code:

`phpunit UnitTest`

> Runs the tests that are provided by the class `UnitTest`. This class
> is expected to be declared in the UnitTest.php sourcefile.
>
> `UnitTest` must be either a class that inherits from
> `PHPUnit\Framework\TestCase` or a class that provides a
> `public static suite()` method which returns a
> `PHPUnit\Framework\Test` object, for example an instance of the
> `PHPUnit\Framework\TestSuite` class.

`phpunit UnitTest UnitTest.php`

> Runs the tests that are provided by the class `UnitTest`. This class
> is expected to be declared in the specified sourcefile.

`--coverage-clover`

> Generates a logfile in XML format with the code coverage information
> for the tests run. See code-coverage-analysis for more details.

`--coverage-crap4j`

> Generates a code coverage report in Crap4j format. See
> code-coverage-analysis for more details.

`--coverage-html`

> Generates a code coverage report in HTML format. See
> code-coverage-analysis for more details.

`--coverage-php`

> Generates a serialized PHP\_CodeCoverage object with the code coverage
> information.

`--coverage-text`

> Generates a logfile or command-line output in human readable format
> with the code coverage information for the tests run.

`--log-junit`

> Generates a logfile in JUnit XML format for the tests run.

`--testdox-html` and `--testdox-text`

> Generates agile documentation in HTML or plain text format for the
> tests that are run (see textui.testdox).

`--filter`

> Only runs tests whose name matches the given regular expression
> pattern. If the pattern is not enclosed in delimiters, PHPUnit will
> enclose the pattern in `/` delimiters.
>
> The test names to match will be in one of the following formats:
>
> `TestNamespace\TestCaseClass::testMethod`
>
> > The default test name format is the equivalent of using the
> > `__METHOD__` magic constant inside the test method.
>
> `TestNamespace\TestCaseClass::testMethod with data set #0`
>
> > When a test has a data provider, each iteration of the data gets the
> > current index appended to the end of the default test name.
>
> `TestNamespace\TestCaseClass::testMethod with data set "my named data"`
>
> > When a test has a data provider that uses named sets, each iteration
> > of the data gets the current name appended to the end of the default
> > test name. See textui.examples.TestCaseClass.php for an example of
> > named data sets.
> >
> >     <?php
> >     use PHPUnit\Framework\TestCase;
> >
> >     namespace TestNamespace;
> >
> >     class TestCaseClass extends TestCase
> >     {
> >         /**
> >          * @dataProvider provider
> >          */
> >         public function testMethod($data)
> >         {
> >             $this->assertTrue($data);
> >         }
> >
> >         public function provider()
> >         {
> >             return [
> >                 'my named data' => [true],
> >                 'my data'       => [true]
> >             ];
> >         }
> >     }
>
> `/path/to/my/test.phpt`
>
> > The test name for a PHPT test is the filesystem path.
>
> See textui.examples.filter-patterns for examples of valid filter
> patterns.
>
>     --filter 'TestNamespace\\TestCaseClass::testMethod'
>     --filter 'TestNamespace\\TestCaseClass'
>     --filter TestNamespace
>     --filter TestCaseClase
>     --filter testMethod
>     --filter '/::testMethod .*"my named data"/'
>     --filter '/::testMethod .*#5$/'
>     --filter '/::testMethod .*#(5|6|7)$/'
>
> See textui.examples.filter-shortcuts for some additional shortcuts
> that are available for matching data providers.
>
>     --filter 'testMethod#2'
>     --filter 'testMethod#2-4'
>     --filter '#2'
>     --filter '#2-4'
>     --filter 'testMethod@my named data'
>     --filter 'testMethod@my.*data'
>     --filter '@my named data'
>     --filter '@my.*data'

`--testsuite`

> Only runs the test suite whose name matches the given pattern.

`--group`

> Only runs tests from the specified group(s). A test can be tagged as
> belonging to a group using the `@group` annotation.
>
> The `@author` and `@ticket` annotations are aliases for `@group`
> allowing to filter tests based on their authors or their ticket
> identifiers, respectively.

`--exclude-group`

> Exclude tests from the specified group(s). A test can be tagged as
> belonging to a group using the `@group` annotation.

`--list-groups`

> List available test groups.

`--test-suffix`

> Only search for test files with specified suffix(es).

`--dont-report-useless-tests`

> Do not report tests that do not test anything. See risky-tests for
> details.

`--strict-coverage`

> Be strict about unintentionally covered code. See risky-tests for
> details.

`--strict-global-state`

> Be strict about global state manipulation. See risky-tests for
> details.

`--disallow-test-output`

> Be strict about output during tests. See risky-tests for details.

`--disallow-todo-tests`

> Does not execute tests which have the `@todo` annotation in its
> docblock.

`--enforce-time-limit`

> Enforce time limit based on test size. See risky-tests for details.

`--process-isolation`

> Run each test in a separate PHP process.

`--no-globals-backup`

> Do not backup and restore $GLOBALS. See fixtures.global-state for more
> details.

`--static-backup`

> Backup and restore static attributes of user-defined classes. See
> fixtures.global-state for more details.

`--colors`

> Use colors in output. On Windows, use
> [ANSICON](https://github.com/adoxa/ansicon) or
> [ConEmu](https://github.com/Maximus5/ConEmu).
>
> There are three possible values for this option:
>
> -
>
> > `never`: never displays colors in the output. This is the default
> > value when `--colors` option is not used.
>
> -
>
> > `auto`: displays colors in the output unless the current terminal
> > doesn't supports colors, or if the output is piped to a command or
> > redirected to a file.
>
> -
>
> > `always`: always displays colors in the output even when the current
> > terminal doesn't supports colors, or when the output is piped to a
> > command or redirected to a file.
>
> When `--colors` is used without any value, `auto` is the chosen value.

`--columns`

> Defines the number of columns to use for progress output. If `max` is
> defined as value, the number of columns will be maximum of the current
> terminal.

`--stderr`

> Optionally print to `STDERR` instead of `STDOUT`.

`--stop-on-error`

> Stop execution upon first error.

`--stop-on-failure`

> Stop execution upon first error or failure.

`--stop-on-risky`

> Stop execution upon first risky test.

`--stop-on-skipped`

> Stop execution upon first skipped test.

`--stop-on-incomplete`

> Stop execution upon first incomplete test.

`--verbose`

> Output more verbose information, for instance the names of tests that
> were incomplete or have been skipped.

`--debug`

> Output debug information such as the name of a test when its execution
> starts.

`--loader`

> Specifies the `PHPUnit\Runner\TestSuiteLoader` implementation to use.
>
> The standard test suite loader will look for the sourcefile in the
> current working directory and in each directory that is specified in
> PHP's `include_path` configuration directive. A class name such as
> `Project_Package_Class` is mapped to the source filename
> Project/Package/Class.php.

`--repeat`

> Repeatedly runs the test(s) the specified number of times.

`--testdox`

> Reports the test progress in TestDox format (see textui.testdox).

`--printer`

> Specifies the result printer to use. The printer class must extend
> `PHPUnit\Util\Printer` and implement the
> `PHPUnit\Framework\TestListener` interface.

`--bootstrap`

> A "bootstrap" PHP file that is run before the tests.

`--configuration`, `-c`

> Read configuration from XML file. See appendixes.configuration for
> more details.
>
> If phpunit.xml or phpunit.xml.dist (in that order) exist in the
> current working directory and `--configuration` is *not* used, the
> configuration will be automatically read from that file.
>
> If a directory is specified and if phpunit.xml or phpunit.xml.dist (in
> that order) exists in this directory, the configuration will be
> automatically read from that file.

`--no-configuration`

> Ignore phpunit.xml and phpunit.xml.dist from the current working
> directory.

`--include-path`

> Prepend PHP's `include_path` with given path(s).

`-d`

> Sets the value of the given PHP configuration option.

Note

Please note that options can be put after the argument(s).

TestDox
-------

PHPUnit's TestDox functionality looks at a test class and all the test
method names and converts them from camel case (or snake\_case) PHP
names to sentences: `testBalanceIsInitiallyZero()` (or
`test_balance_is_initially_zero()` becomes "Balance is initially zero".
If there are several test methods whose names only differ in a suffix of
one or more digits, such as `testBalanceCannotBecomeNegative()` and
`testBalanceCannotBecomeNegative2()`, the sentence "Balance cannot
become negative" will appear only once, assuming that all of these tests
succeed.

Let us take a look at the agile documentation generated for a
`BankAccount` class:

Alternatively, the agile documentation can be generated in HTML or plain
text format and written to a file using the `--testdox-html` and
`--testdox-text` arguments.

Agile Documentation can be used to document the assumptions you make
about the external packages that you use in your project. When you use
an external package, you are exposed to the risks that the package will
not behave as you expect, and that future versions of the package will
change in subtle ways that will break your code, without you knowing it.
You can address these risks by writing a test every time you make an
assumption. If your test succeeds, your assumption is valid. If you
document all your assumptions with tests, future releases of the
external package will be no cause for concern: if the tests succeed,
your system should continue working.
