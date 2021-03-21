The XML Configuration File
==========================

The `<phpunit>` Element
-----------------------

### The `backupGlobals` Attribute

Possible values: `true` or `false` (default: `false`)

PHPUnit can optionally backup all global and super-global variables
before each test and restore this backup after each test.

This attribute configures this operation for all tests. This
configuration can be overridden using the `@backupGlobals` annotation on
the test case class and test method level.

### The `backupStaticAttributes` Attribute

Possible values: `true` or `false` (default: `false`)

PHPUnit can optionally backup all static attributes in all declared
classes before each test and restore this backup after each test.

This attribute configures this operation for all tests. This
configuration can be overridden using the `@backupStaticAttributes`
annotation on the test case class and test method level.

### The `bootstrap` Attribute

This attribute configures the bootstrap script that is loaded before the
tests are executed. This script usually only registers the autoloader
callback that is used to load the code under test.

### The `cacheResult` Attribute

Possible values: `true` or `false` (default: `true`)

This attribute configures the caching of test results. This caching is
required for certain other features to work.

### The `cacheResultFile` Attribute

This attribute configures the file in which the test result cache (see
above) is stored.

### The `colors` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether colors are used in PHPUnit's output.

Setting this attribute to `true` is equivalent to using the
`--colors=auto` CLI option.

Setting this attribute to `false` is equivalent to using the
`--colors=never` CLI option.

### The `columns` Attribute

Possible values: integer or string `max` (default: `80`)

This attribute configures the number of columns to use for progress
output.

If `max` is defined as value, the number of columns will be maximum of
the current terminal.

### The `convertDeprecationsToExceptions` Attribute

Possible values: `true` or `false` (default: `true`)

This attribute configures whether `E_DEPRECATED` and `E_USER_DEPRECATED`
events triggered by the code under test are converted to an exception
(and mark the test as error).

### The `convertErrorsToExceptions` Attribute

Possible values: `true` or `false` (default: `true`)

This attribute configures whether `E_ERROR` and `E_USER_ERROR` events
triggered by the code under test are converted to an exception (and mark
the test as error).

### The `convertNoticesToExceptions` Attribute

Possible values: `true` or `false` (default: `true`)

This attribute configures whether `E_STRICT`, `E_NOTICE`, and
`E_USER_NOTICE` events triggered by the code under test are converted to
an exception (and mark the test as error).

### The `convertWarningsToExceptions` Attribute

Possible values: `true` or `false` (default: `true`)

This attribute configures whether `E_WARNING` and `E_USER_WARNING`
events triggered by the code under test are converted to an exception
(and mark the test as error).

### The `forceCoversAnnotation` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether a test will be marked as risky (see
`risky-tests.unintentionally-covered-code`) when it does not have a
`@covers <appendixes.annotations.covers>` annotation.

### The `printerClass` Attribute

Default: `PHPUnit\TextUI\ResultPrinter`

This attribute configures the name of a class that either is
`PHPUnit\TextUI\ResultPrinter` or that extends
`PHPUnit\TextUI\ResultPrinter`. An object of this class is used to print
progress and test results.

### The `printerFile` Attribute

This attribute can be used to configure the path to the sourcecode file
that declares the class configured with `printerClass` in case that
class cannot be autoloaded.

### The `processIsolation` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether each test should be run in a separate
PHP process for increased isolation.

### The `stopOnError` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the test suite execution should be
stopped after the first test finished with status "error".

### The `stopOnFailure` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the test suite execution should be
stopped after the first test finished with status "failure".

### The `stopOnIncomplete` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the test suite execution should be
stopped after the first test finished with status "incomplete".

### The `stopOnRisky` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the test suite execution should be
stopped after the first test finished with status "risky".

### The `stopOnSkipped` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the test suite execution should be
stopped after the first test finished with status "skipped".

### The `stopOnWarning` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the test suite execution should be
stopped after the first test finished with status "warning".

### The `stopOnDefect` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the test suite execution should be
stopped after the first test finished with a status "error", "failure",
"risky" or "warning".

### The `failOnRisky` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the PHPUnit test runner should exit
with a shell exit code that indicates failure when all tests are
successful but there are tests that were marked as risky.

### The `failOnWarning` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the PHPUnit test runner should exit
with a shell exit code that indicates failure when all tests are
successful but there are tests that had warnings.

### The `beStrictAboutChangesToGlobalState` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether PHPUnit should mark a test as risky
when global state is manipulated by the code under test (or the test
code).

### The `beStrictAboutOutputDuringTests` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether PHPUnit should mark a test as risky
when the code under test (or the test code) prints output.

### The `beStrictAboutResourceUsageDuringSmallTests` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether PHPUnit should mark a test that is
annotated with `@small` as risky when it invokes a PHP built-in function
or method that operates on `resource` variables.

### The `beStrictAboutTestsThatDoNotTestAnything` Attribute

Possible values: `true` or `false` (default: `true`)

This attribute configures whether PHPUnit should mark a test as risky
when no assertions are performed (expectations are also considered).

### The `beStrictAboutTodoAnnotatedTests` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether PHPUnit should mark a test as risky
when it is annotated with `@todo`.

### The `beStrictAboutCoversAnnotation` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether PHPUnit should mark a test as risky
when it executes code that is not specified using `@covers` or `@uses`.

### The `enforceTimeLimit` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether time limits should be enforced.

### The `defaultTimeLimit` Attribute

Possible values: integer (default: `0`)

This attribute configures the default time limit (in seconds).

### The `timeoutForSmallTests` Attribute

Possible values: integer (default: `1`)

This attribute configures the time limit for tests annotated with
`@small` (in seconds).

### The `timeoutForMediumTests` Attribute

Possible values: integer (default: `10`)

This attribute configures the time limit for tests annotated with
`@medium` (in seconds).

### The `timeoutForLargeTests` Attribute

Possible values: integer (default: `60`)

This attribute configures the time limit for tests annotated with
`@large` (in seconds).

### The `testSuiteLoaderClass` Attribute

Default: `PHPUnit\Runner\StandardTestSuiteLoader`

This attribute configures the name of a class that implements the
`PHPUnit\Runner\TestSuiteLoader` interface. An object of this class is
used to load the test suite.

### The `testSuiteLoaderFile` Attribute

This attribute can be used to configure the path to the sourcecode file
that declares the class configured with `testSuiteLoaderClass` in case
that class cannot be autoloaded.

### The `defaultTestSuite` Attribute

This attribute configures the name of the default test suite.

### The `verbose` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether more verbose output should be printed.

### The `stderr` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether PHPUnit should print its output to
`stderr` instead of `stdout`.

### The `reverseDefectList` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether tests that are not successful should
be printed in reverse order.

### The `registerMockObjectsFromTestArgumentsRecursively` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether arrays and object graphs that are
passed from one test to another using the `@depends` annotation should
be recursively scanned for mock objects.

### The `extensionsDirectory` Attribute

When `phpunit.phar` is used then this attribute may be used to configure
a directory from which all `*.phar` files will be loaded as extensions
for the PHPUnit test runner.

### The `executionOrder` Attribute

Possible values: `default`, `defects`, `depends`, `no-depends`,
`duration`, `random`, `reverse`, `size`

Using multiple values is possible. These need to be separated by `,`.

This attribute configures the order in which tests are executed.

### The `resolveDependencies` Attribute

Possible values: `true` or `false` (default: `true`)

This attribute configures whether dependencies between tests (expressed
using the `@depends` annotation) should be resolved.

### The `testdox` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the output should be printed in
TestDox format.

### The `noInteraction` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether progress should be animated when
TestDox format is used, for instance.

The `<testsuites>` Element
--------------------------

Parent element: `<phpunit>`

This element is the root for one or more `<testsuite>` elements that are
used to configure the tests that are to be executed.

### The `<testsuite>` Element

Parent element: `<testsuites>`

A `<testsuite>` element must have a `name` attribute and may have one or
more `<directory>` and/or `<file>` child elements that configure
directories and/or files, respectively, that should be searched for
tests.

    <testsuites>
      <testsuite name="unit">
        <directory>tests/unit</directory>
      </testsuite>

      <testsuite name="integration">
        <directory>tests/integration</directory>
      </testsuite>

      <testsuite name="edge-to-edge">
        <directory>tests/edge-to-edge</directory>
      </testsuite>
    </testsuites>

Using the `phpVersion` and `phpVersionOperator` attributes, a required
PHP version can be specified:

    <testsuites>
      <testsuite name="unit">
        <directory phpVersion="8.0.0" phpVersionOperator=">=">tests/unit</directory>
      </testsuite>
    </testsuites>

In the example above, the tests from the `tests/unit` directory are only
added to the test suite if the PHP version is at least 8.0.0. The
`phpVersionOperator` attribute is optional and defaults to `>=`.

The `<coverage>` Element
------------------------

Parent element: `<phpunit>`

The `<coverage>` element and its children can be used to configure code
coverage:

    <coverage cacheDirectory="/path/to/directory"
              includeUncoveredFiles="true"
              processUncoveredFiles="true"
              pathCoverage="false"
              ignoreDeprecatedCodeUnits="true"
              disableCodeCoverageIgnore="true">
        <!-- ... -->
    </coverage>

### The `cacheDirectory` Attribute

Possible values: string

When code coverage data is collected and processed, static code analysis
is performed to improve reasoning about the covered code. This is an
expensive operation, whose result can be cached. When the
`cacheDirectory` attribute is set, static analysis results will be
cached in the specified directory.

### The `includeUncoveredFiles` Attribute

Possible values: `true` or `false` (default: `true`)

When set to `true`, all sourcecode files that are configured to be
considered for code coverage analysis will be included in the code
coverage report(s). This includes sourcecode files that are not executed
while the tests are running.

### The `processUncoveredFiles` Attribute

Possible values: `true` or `false` (default: `false`)

When set to `true`, all sourcecode files that are configured to be
considered for code coverage analysis will be processed. This includes
sourcecode files that are not executed while the tests are running.

### The `ignoreDeprecatedCodeUnits` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether code units annotated with
`@deprecated` should be ignored from code coverage.

### The `pathCoverage` Attribute

Possible values: `true` or `false` (default: `false`)

When set to `false`, only line coverage data will be collected,
processed, and reported.

When set to `true`, line coverage, branch coverage, and path coverage
data will be collected, processed, and reported. This requires a code
coverage driver that supports path coverage. Path Coverage is currently
only implemented by Xdebug.

### The `disableCodeCoverageIgnore` Attribute

Possible values: `true` or `false` (default: `false`)

This attribute configures whether the `@codeCoverageIgnore*` annotations
should be ignored.

### The `<include>` Element

Parent element: `<coverage>`

Configures a set of files to be included in code coverage report(s).

    <include>
        <directory suffix=".php">src</directory>
    </include>

The example shown above instructs PHPUnit to include all sourcecode
files with `.php` suffix in the `src` directory and its sub-directories
in the code coverage report(s).

### The `<exclude>` Element

Parent element: `<coverage>`

Configures a set of files to be excluded from code coverage report(s).

    <include>
        <directory suffix=".php">src</directory>
    </include>

    <exclude>
        <directory suffix=".php">src/generated</directory>
        <file>src/autoload.php</file>
    </exclude>

The example shown above instructs PHPUnit to include all sourcecode
files with `.php` suffix in the `src` directory and its sub-directories
in the code coverage report but exclude all files with `.php` suffix in
the `src/generated` directory and its sub-directories as well as the
`src/autoload.php` file from the code coverage report(s).

### The `<directory>` Element

Parent elements: `<include>`, `<exclude>`

Configures a directory and its sub-directories for inclusion in or
exclusion from code coverage report(s).

#### The `prefix` Attribute

Possible values: string

Configures a prefix-based filter that is applied to the names of files
in the directory and its sub-directories.

#### The `suffix` Attribute

Possible values: string (default: `'.php'`)

Configures a suffix-based filter that is applied to the names of files
in the directory and its sub-directories.

#### The `phpVersion` Attribute

Possible values: string

Configures a filter based on the version of the PHP runtime that is used
to run the current PHPUnit process.

#### The `phpVersionOperator` Attribute

Possible values: `'<'`, `'lt'`, `'<='`, `'le'`, `'>'`, `'gt'`, `'>='`,
`'ge'`, `'=='`, `'='`, `'eq'`, `'!='`, `'<>'`, `'ne'` (default: `'>='`)

Configures the comparison operator to be used with `version_compare()`
for the filter based on the version of the PHP runtime that is used to
run the current PHPUnit process.

### The `<file>` Element

Parent elements: `<include>`, `<exclude>`

Configures a file for inclusion in or exclusion from code coverage
report(s).

### The `<report>` Element

Parent element: `<coverage>`

Configures the code coverage reports to be generated.

    <report>
        <clover outputFile="clover.xml"/>
        <crap4j outputFile="crap4j.xml" threshold="50"/>
        <html outputDirectory="html-coverage" lowUpperBound="50" highLowerBound="90"/>
        <php outputFile="coverage.php"/>
        <text outputFile="coverage.txt" showUncoveredFiles="false" showOnlySummary="true"/>
        <xml outputDirectory="xml-coverage"/>
    </report>

#### The `<clover>` Element

Parent element: `<report>`

Configures a code coverage report in Clover XML format.

##### The `outputFile` Attribute

Possible values: string

The file to which the Clover XML report is written.

#### The `<crap4j>` Element

Parent element: `<report>`

Configures a code coverage report in Crap4J XML format.

##### The `outputFile` Attribute

Possible values: string

The file to which the Crap4J XML report is written.

##### The `threshold` Attribute

Possible values: integer (default: `50`)

#### The `<html>` Element

Parent element: `<report>`

Configures a code coverage report in HTML format.

##### The `outputDirectory` Attribute

The directory to which the HTML report is written.

##### The `lowUpperBound` Attribute

Possible values: integer (default: `50`)

The upper bound of what should be considered "low coverage".

##### The `highLowerBound` Attribute

Possible values: integer (default: `90`)

The lower bound of what should be considered "high coverage".

#### The `<php>` Element

Parent element: `<report>`

Configures a code coverage report in PHP format.

##### The `outputFile` Attribute

Possible values: string

The file to which the PHP report is written.

#### The `<text>` Element

Parent element: `<report>`

Configures a code coverage report in text format.

##### The `outputFile` Attribute

Possible values: string

The file to which the text report is written.

##### The `showUncoveredFiles` Attribute

Possible values: `true` or `false` (default: `false`)

##### The `showOnlySummary` Attribute

Possible values: `true` or `false` (default: `false`)

#### The `<xml>` Element

Parent element: `<report>`

Configures a code coverage report in PHPUnit XML format.

##### The `outputDirectory` Attribute

Possible values: string

The directory to which the PHPUnit XML report is written.

The `<logging>` Element
-----------------------

Parent element: `<phpunit>`

The `<logging>` element and its children can be used to configure the
logging of the test execution.

    <logging>
        <junit outputFile="junit.xml"/>
        <teamcity outputFile="teamcity.txt"/>
        <testdoxHtml outputFile="testdox.html"/>
        <testdoxText outputFile="testdox.txt"/>
        <testdoxXml outputFile="testdox.xml"/>
        <text outputFile="logfile.txt"/>
    </logging>

### The `<junit>` Element

Parent element: `<logging>`

Configures a test result logfile in JUnit XML format.

#### The `outputFile` Attribute

Possible values: string

The file to which the test result logfile in JUnit XML format is
written.

### The `<teamcity>` Element

Parent element: `<logging>`

Configures a test result logfile in TeamCity format.

#### The `outputFile` Attribute

Possible values: string

The file to which the test result logfile in TeamCity format is written.

### The `<testdoxHtml>` Element

Parent element: `<logging>`

Configures a test result logfile in TestDox HTML format.

#### The `outputFile` Attribute

Possible values: string

The file to which the test result logfile in TestDox HTML format is
written.

### The `<testdoxText>` Element

Parent element: `<logging>`

Configures a test result logfile in TestDox text format.

#### The `outputFile` Attribute

Possible values: string

The file to which the test result logfile in TestDox text format is
written.

### The `<testdoxXml>` Element

Parent element: `<logging>`

Configures a test result logfile in TestDox XML format.

#### The `outputFile` Attribute

Possible values: string

The file to which the test result logfile in TestDox XML format is
written.

### The `<text>` Element

Parent element: `<logging>`

Configures a test result logfile in text format.

#### The `outputFile` Attribute

Possible values: string

The file to which the test result logfile in text format is written.

The `<groups>` Element
----------------------

Parent element: `<phpunit>`

The `<groups>` element and its `<include>`, `<exclude>`, and `<group>`
children can be used to select groups of tests marked with the `@group`
annotation (documented in `appendixes.annotations.group`) that should
(not) be run:

    <groups>
      <include>
        <group>name</group>
      </include>
      <exclude>
        <group>name</group>
      </exclude>
    </groups>

The example shown above is equivalent to invoking the PHPUnit test
runner with `--group name --exclude-group name`.

The `<testdoxGroups>` Element
-----------------------------

Parent element: `<phpunit>`

... TBD ...

The `<listeners>` Element
-------------------------

Parent element: `<phpunit>`

The `<listeners>` element and its `<listener>` children can be used to
attach additional test listeners to the test execution.

### The `<listener>` Element

Parent element: `<listeners>`

    <listeners>
      <listener class="MyListener" file="/optional/path/to/MyListener.php">
        <arguments>
          <array>
            <element key="0">
              <string>Sebastian</string>
            </element>
          </array>
          <integer>22</integer>
          <string>April</string>
          <double>19.78</double>
          <null/>
          <object class="stdClass"/>
        </arguments>
      </listener>
    </listeners>

The XML configuration above corresponds to attaching the `$listener`
object (see below) to the test execution:

    $listener = new MyListener(
        ['Sebastian'],
        22,
        'April',
        19.78,
        null,
        new stdClass
    );

Note

Please note that the `PHPUnit\Framework\TestListener` interface is
deprecated and will be removed in the future. TestRunner extensions
should be used instead of test listeners.

The `<extensions>` Element
--------------------------

Parent element: `<phpunit>`

The `<extensions>` element and its `<extension>` children can be used to
register test runner extensions.

### The `<extension>` Element

Parent element: `<extensions>`

    <extensions>
        <extension class="Vendor\MyExtension"/>
    </extensions>

#### The `<arguments>` Element

Parent element: `<extension>`

The `<arguments>` element can be used to configure a single
`<extension>`.

Accepts a list of elements of types, which are then used to configure
individual extensions. The arguments are passed to the extension class'
`__constructor` method in the order they are defined in the
configuration.

Available types:

-   `<boolean>`
-   `<integer>`
-   `<string>`
-   `<double>` (float)
-   `<array>`
-   `<object>`

<!-- -->

    <extension class="Vendor\MyExtension">
        <arguments>
            <integer>1</integer>
            <integer>2</integer>
            <integer>3</integer>
            <string>hello world</string>
            <boolean>true</boolean>
            <double>1.23</double>
            <array>
                <element index="0">
                    <string>value1</string>
                </element>
                <element index="1">
                    <string>value2</string>
                </element>
            </array>
            <object class="Vendor\MyPhpClass">
                <string>constructor arg 1</string>
                <string>constructor arg 2</string>
            </object>
        </arguments>
    </extension>

The `<php>` Element
-------------------

Parent element: `<phpunit>`

The `<php>` element and its children can be used to configure PHP
settings, constants, and global variables. It can also be used to
prepend the `include_path`.

### The `<includePath>` Element

Parent element: `<php>`

This element can be used to prepend a path to the `include_path`.

### The `<ini>` Element

Parent element: `<php>`

This element can be used to set a PHP configuration setting.

    <php>
      <ini name="foo" value="bar"/>
    </php>

The XML configuration above corresponds to the following PHP code:

    ini_set('foo', 'bar');

### The `<const>` Element

Parent element: `<php>`

This element can be used to set a global constant.

    <php>
      <const name="foo" value="bar"/>
    </php>

The XML configuration above corresponds to the following PHP code:

    define('foo', 'bar');

### The `<var>` Element

Parent element: `<php>`

This element can be used to set a global variable.

    <php>
      <var name="foo" value="bar"/>
    </php>

The XML configuration above corresponds to the following PHP code:

    $GLOBALS['foo'] = 'bar';

### The `<env>` Element

Parent element: `<php>`

This element can be used to set a value in the super-global array
`$_ENV`.

    <php>
      <env name="foo" value="bar"/>
    </php>

The XML configuration above corresponds to the following PHP code:

    $_ENV['foo'] = 'bar';

By default, environment variables are not overwritten if they exist
already. To force overwriting existing variables, use the `force`
attribute:

    <php>
      <env name="foo" value="bar" force="true"/>
    </php>

### The `<get>` Element

Parent element: `<php>`

This element can be used to set a value in the super-global array
`$_GET`.

    <php>
      <get name="foo" value="bar"/>
    </php>

The XML configuration above corresponds to the following PHP code:

    $_GET['foo'] = 'bar';

### The `<post>` Element

Parent element: `<php>`

This element can be used to set a value in the super-global array
`$_POST`.

    <php>
      <post name="foo" value="bar"/>
    </php>

The XML configuration above corresponds to the following PHP code:

    $_POST['foo'] = 'bar';

### The `<cookie>` Element

Parent element: `<php>`

This element can be used to set a value in the super-global array
`$_COOKIE`.

    <php>
      <cookie name="foo" value="bar"/>
    </php>

The XML configuration above corresponds to the following PHP code:

    $_COOKIE['foo'] = 'bar';

### The `<server>` Element

Parent element: `<php>`

This element can be used to set a value in the super-global array
`$_SERVER`.

    <php>
      <server name="foo" value="bar"/>
    </php>

The XML configuration above corresponds to the following PHP code:

    $_SERVER['foo'] = 'bar';

### The `<files>` Element

Parent element: `<php>`

This element can be used to set a value in the super-global array
`$_FILES`.

    <php>
      <files name="foo" value="bar"/>
    </php>

The XML configuration above corresponds to the following PHP code:

    $_FILES['foo'] = 'bar';

### The `<request>` Element

Parent element: `<php>`

This element can be used to set a value in the super-global array
`$_REQUEST`.

    <php>
      <request name="foo" value="bar"/>
    </php>

The XML configuration above corresponds to the following PHP code:

    $_REQUEST['foo'] = 'bar';
