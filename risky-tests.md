Risky Tests
===========

PHPUnit can perform the additional checks documented below while it
executes the tests.

Useless Tests
-------------

PHPUnit is by default strict about tests that do not test anything. This
check can be disabled by using the `--dont-report-useless-tests` option
on the `command line <textui.clioptions>` or by setting
`beStrictAboutTestsThatDoNotTestAnything="false"` in PHPUnit's
`configuration file <appendixes.configuration>`.

A test that does not perform an assertion will be marked as risky when
this check is enabled. Expectations on mock objects count as an
assertion.

Unintentionally Covered Code
----------------------------

PHPUnit can be strict about unintentionally covered code. This check can
be enabled by using the `--strict-coverage` option on the
`command line <textui.clioptions>` or by setting
`beStrictAboutCoversAnnotation="true"` in PHPUnit's
`configuration file <appendixes.configuration>`.

A test that is annotated with `@covers <appendixes.annotations.covers>`
and executes code that is not listed using a
`@covers <appendixes.annotations.covers>` or
`@uses <appendixes.annotations.uses>` annotation will be marked as risky
when this check is enabled.

Furthermore, by setting `forceCoversAnnotation="true"` in PHPUnit's
`configuration file <appendixes.configuration>`, a test can be marked as
risky when it does not have a `@covers <appendixes.annotations.covers>`
annotation.

Output During Test Execution
----------------------------

PHPUnit can be strict about output during tests. This check can be
enabled by using the `--disallow-test-output` option on the
`command line <textui.clioptions>` or by setting
`beStrictAboutOutputDuringTests="true"` in PHPUnit's
`configuration file <appendixes.configuration>`.

A test that emits output, for instance by invoking print in either the
test code or the tested code, will be marked as risky when this check is
enabled.

Test Execution Timeout
----------------------

A time limit can be enforced for the execution of a test if the
[PHP\_Invoker](https://packagist.org/packages/phpunit/php-invoker)
package is installed and the `pcntl` extension is available. The
enforcing of this time limit can be enabled by using the
`--enforce-time-limit` option on the `command line <textui.clioptions>`
or by setting `enforceTimeLimit="true"` in PHPUnit's
`configuration file <appendixes.configuration>`.

A test annotated with `@large` will be marked as risky if it takes
longer than 60 seconds to execute. This timeout is configurable via the
`timeoutForLargeTests` attribute in the
`configuration file <appendixes.configuration>`.

A test annotated with `@medium` will be marked as risky if it takes
longer than 10 seconds to execute. This timeout is configurable via the
`timeoutForMediumTests` attribute in the configuration
`configuration file <appendixes.configuration>`.

A test annotated with `@small` will be marked as risky if it takes
longer than 1 second to execute. This timeout is configurable via the
`timeoutForSmallTests` attribute in the
`configuration file <appendixes.configuration>`.

Note

Tests need to be explicitly annotated by either `@small`, `@medium`, or
`@large` to enable run time limits.

To exit the test run with a non-zero exit code when tests overrun their
time-limit, the `--fail-on-risky` option on the
`command line <textui.clioptions>` or the `failOnRisky="true"` setting
in PHPUnit's `configuration file <appendixes.configuration>` needs to be
enabled.

Global State Manipulation
-------------------------

PHPUnit can be strict about tests that manipulate global state. This
check can be enabled by using the `--strict-global-state` option on the
`command line <textui.clioptions>` or by setting
`beStrictAboutChangesToGlobalState="true"` in PHPUnit's
`configuration file <appendixes.configuration>`.
