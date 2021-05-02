Incomplete and Skipped Tests
============================

Incomplete Tests
----------------

When you are working on a new test case class, you might want to begin
by writing empty test methods such as:

    public function testSomething(): void
    {
    }

to keep track of the tests that you have to write. The problem with
empty test methods is that they are interpreted as a success by the
PHPUnit framework. This misinterpretation leads to the test reports
being useless -- you cannot see whether a test is actually successful or
just not yet implemented. Calling `$this->fail()` in the unimplemented
test method does not help either, since then the test will be
interpreted as a failure. This would be just as wrong as interpreting an
unimplemented test as a success.

If we think of a successful test as a green light and a test failure as
a red light, we need an additional yellow light to mark a test as being
incomplete or not yet implemented. `PHPUnit\Framework\IncompleteTest` is
a marker interface for marking an exception that is raised by a test
method as the result of the test being incomplete or currently not
implemented. `PHPUnit\Framework\IncompleteTestError` is the standard
implementation of this interface.

`incomplete-and-skipped-tests.incomplete-tests.examples.SampleTest.php`
shows a test case class, `SampleTest`, that contains one test method,
`testSomething()`. By calling the convenience method
`markTestIncomplete()` (which automatically raises an
`PHPUnit\Framework\IncompleteTestError` exception) in the test method,
we mark the test as being incomplete.

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class SampleTest extends TestCase
    {
        public function testSomething(): void
        {
            // Optional: Test anything here, if you want.
            $this->assertTrue(true, 'This should already work.');

            // Stop here and mark this test as incomplete.
            $this->markTestIncomplete(
              'This test has not been implemented yet.'
            );
        }
    }

An incomplete test is denoted by an `I` in the output of the PHPUnit
command-line test runner, as shown in the following example:

$ phpunit --verbose SampleTest PHPUnit .0 by Sebastian Bergmann and
contributors.

I

Time: 0 seconds, Memory: 3.95Mb

There was 1 incomplete test:

1) SampleTest::testSomething This test has not been implemented yet.

/home/sb/SampleTest.php:12 OK, but incomplete or skipped tests! Tests:
1, Assertions: 1, Incomplete: 1.

`incomplete-and-skipped-tests.incomplete-tests.tables.api` shows the API
for marking tests as incomplete.

table

<table>
<caption>API for Incomplete Tests</caption>
<thead>
<tr class="header">
<th>Method</th>
<th>Meaning</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>void markTestIncomplete()</code></td>
<td>Marks the current test as incomplete.</td>
</tr>
<tr class="even">
<td><code>void markTestIncomplete(string $message)</code></td>
<td>Marks the current test as incomplete using <code>$message</code> as an explanatory message.</td>
</tr>
</tbody>
</table>

Skipping Tests
--------------

Not all tests can be run in every environment. Consider, for instance, a
database abstraction layer that has several drivers for the different
database systems it supports. The tests for the MySQL driver can only be
run if a MySQL server is available.

`incomplete-and-skipped-tests.skipping-tests.examples.DatabaseTest.php`
shows a test case class, `DatabaseTest`, that contains one test method,
`testConnection()`. In the test case class' `setUp()` template method we
check whether the MySQLi extension is available and use the
`markTestSkipped()` method to skip the test if it is not.

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class DatabaseTest extends TestCase
    {
        protected function setUp(): void
        {
            if (!extension_loaded('mysqli')) {
                $this->markTestSkipped(
                  'The MySQLi extension is not available.'
                );
            }
        }

        public function testConnection(): void
        {
            // ...
        }
    }

A test that has been skipped is denoted by an `S` in the output of the
PHPUnit command-line test runner, as shown in the following example:

$ phpunit --verbose DatabaseTest PHPUnit .0 by Sebastian Bergmann and
contributors.

S

Time: 0 seconds, Memory: 3.95Mb

There was 1 skipped test:

1) DatabaseTest::testConnection The MySQLi extension is not available.

/home/sb/DatabaseTest.php:9 OK, but incomplete or skipped tests! Tests:
1, Assertions: 0, Skipped: 1.

`incomplete-and-skipped-tests.skipped-tests.tables.api` shows the API
for skipping tests.

table

<table>
<caption>API for Skipping Tests</caption>
<thead>
<tr class="header">
<th>Method</th>
<th>Meaning</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>void markTestSkipped()</code></td>
<td>Marks the current test as skipped.</td>
</tr>
<tr class="even">
<td><code>void markTestSkipped(string $message)</code></td>
<td>Marks the current test as skipped using <code>$message</code> as an explanatory message.</td>
</tr>
</tbody>
</table>

Skipping Tests using @requires
------------------------------

In addition to the above methods it is also possible to use the
`@requires` annotation to express common preconditions for a test case.

A test can have multiple `@requires` annotations, in which case all
requirements need to be met for the test to run.

table

<table>
<caption>Possible @requires usages</caption>
<thead>
<tr class="header">
<th>Type</th>
<th>Possible Values</th>
<th>Examples</th>
<th>Another example</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>PHP</code></td>
<td>Any PHP version identifier along with an optional operator</td>
<td>@requires PHP 7.1.20</td>
<td>@requires PHP &gt;= 7.2</td>
</tr>
<tr class="even">
<td><code>PHPUnit</code></td>
<td>Any PHPUnit version identifier along with an optional operator</td>
<td>@requires PHPUnit 7.3.1</td>
<td>@requires PHPUnit &lt; 8</td>
</tr>
<tr class="odd">
<td><code>OS</code></td>
<td>A regexp matching <a href="https://www.php.net/manual/en/reserved.constants.php#constant.php-os">PHP_OS</a></td>
<td>@requires OS Linux</td>
<td>@requires OS WIN32|WINNT</td>
</tr>
<tr class="even">
<td><code>OSFAMILY</code></td>
<td>Any <a href="https://www.php.net/manual/en/reserved.constants.php#constant.php-os-family">OS family</a></td>
<td>@requires OSFAMILY Solaris</td>
<td>@requires OSFAMILY Windows</td>
</tr>
<tr class="odd">
<td><code>function</code></td>
<td>Any valid parameter to <a href="https://www.php.net/function_exists">function_exists</a></td>
<td>@requires function imap_open</td>
<td>@requires function ReflectionMethod::setAccessible</td>
</tr>
<tr class="even">
<td><code>extension</code></td>
<td>Any extension name along with an optional version identifier and optional operator</td>
<td>@requires extension mysqli</td>
<td>@requires extension redis &gt;= 2.2.0</td>
</tr>
</tbody>
</table>

The following operators are supported for PHP, PHPUnit, and extension
version constraints: `<`, `<=`, `>`, `>=`, `=`, `==`, `!=`, `<>`.

Versions are compared using PHP's
[version\_compare](https://www.php.net/version_compare) function. Among
other things, this means that the `=` and `==` operator can only be used
with complete `X.Y.Z` version numbers and that just `X.Y` will not work.

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    /**
     * @requires extension mysqli
     */
    final class DatabaseTest extends TestCase
    {
        /**
         * @requires PHP >= 5.3
         */
        public function testConnection(): void
        {
            // Test requires the mysqli extension and PHP >= 5.3
        }

        // ... All other tests require the mysqli extension
    }

If you are using syntax that doesn't compile with a certain PHP Version
look into the xml configuration for version dependent includes in
`appendixes.configuration.testsuites`
