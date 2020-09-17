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

incomplete-and-skipped-tests.incomplete-tests.examples.SampleTest.php
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

incomplete-and-skipped-tests.incomplete-tests.tables.api shows the API
for marking tests as incomplete.

Skipping Tests
--------------

Not all tests can be run in every environment. Consider, for instance, a
database abstraction layer that has several drivers for the different
database systems it supports. The tests for the MySQL driver can only be
run if a MySQL server is available.

incomplete-and-skipped-tests.skipping-tests.examples.DatabaseTest.php
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

incomplete-and-skipped-tests.skipped-tests.tables.api shows the API for
skipping tests.

Skipping Tests using @requires
------------------------------

In addition to the above methods it is also possible to use the
`@requires` annotation to express common preconditions for a test case.

The following operators are supported for PHP, PHPUnit, and extension
version constraints: `<`, `<=`, `>`, `>=`, `=`, `==`, `!=`, `<>`.

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
appendixes.configuration.testsuites
