Writing Tests for PHPUnit
=========================

writing-tests-for-phpunit.examples.StackTest.php shows how we can write
tests using PHPUnit that exercise PHP's array operations. The example
introduces the basic conventions and steps for writing tests with
PHPUnit:

\#.

> The tests for a class `Class` go into a class `ClassTest`.

\#.

> `ClassTest` inherits (most of the time) from
> `PHPUnit\Framework\TestCase`.

\#.

> The tests are public methods that are named `test*`.
>
> Alternatively, you can use the `@test` annotation in a method's
> docblock to mark it as a test method.

\#.

> Inside the test methods, assertion methods such as `assertSame()` (see
> appendixes.assertions) are used to assert that an actual value matches
> an expected value.

    <?php
    use PHPUnit\Framework\TestCase;

    class StackTest extends TestCase
    {
        public function testPushAndPop()
        {
            $stack = [];
            $this->assertSame(0, count($stack));

            array_push($stack, 'foo');
            $this->assertSame('foo', $stack[count($stack)-1]);
            $this->assertSame(1, count($stack));

            $this->assertSame('foo', array_pop($stack));
            $this->assertSame(0, count($stack));
        }
    }

|  
*Martin Fowler*:

Whenever you are tempted to type something into a `print` statement or a
debugger expression, write it as a test instead.

Test Dependencies
-----------------

> *Adrian Kuhn et. al.*:
>
> Unit Tests are primarily written as a good practice to help developers
> identify and fix bugs, to refactor code and to serve as documentation
> for a unit of software under test. To achieve these benefits, unit
> tests ideally should cover all the possible paths in a program. One
> unit test usually covers one specific path in one function or method.
> However a test method is not necessarily an encapsulated, independent
> entity. Often there are implicit dependencies between test methods,
> hidden in the implementation scenario of a test.

PHPUnit supports the declaration of explicit dependencies between test
methods. Such dependencies do not define the order in which the test
methods are to be executed but they allow the returning of an instance
of the test fixture by a producer and passing it to the dependent
consumers.

<table>
<tbody>
<tr class="odd">
<td>A producer is a test method that yields its unit under test as return value.</td>
</tr>
<tr class="even">
<td>-</td>
</tr>
<tr class="odd">
<td>A consumer is a test method that depends on one or more producers and their return values.</td>
</tr>
<tr class="even">
<td>writing-tests-for-phpunit.examples.StackTest2.php shows</td>
</tr>
<tr class="odd">
<td>how to use the <code>@depends</code> annotation to express</td>
</tr>
<tr class="even">
<td>dependencies between test methods.</td>
</tr>
<tr class="odd">
<td>.. code-block:: php</td>
</tr>
<tr class="even">
<td>:caption: Using the <code>@depends</code> annotation to express dependencies</td>
</tr>
<tr class="odd">
<td>:name: writing-tests-for-phpunit.examples.StackTest2.php</td>
</tr>
<tr class="even">
<td>&lt;?php</td>
</tr>
<tr class="odd">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="even">
<td>class StackTest extends TestCase</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>public function testEmpty()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$stack = [];</td>
</tr>
<tr class="odd">
<td>$this-&gt;assertEmpty($stack);</td>
</tr>
<tr class="even">
<td>return $stack;</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>/**</td>
</tr>
<tr class="odd">
<td>* @depends testEmpty</td>
</tr>
<tr class="even">
<td>*/</td>
</tr>
<tr class="odd">
<td>public function testPush(array $stack)</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>array_push($stack, 'foo');</td>
</tr>
<tr class="even">
<td>$this-&gt;assertSame('foo', $stack[count($stack)-1]);</td>
</tr>
<tr class="odd">
<td>$this-&gt;assertNotEmpty($stack);</td>
</tr>
<tr class="even">
<td>return $stack;</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>/**</td>
</tr>
<tr class="odd">
<td>* @depends testPush</td>
</tr>
<tr class="even">
<td>*/</td>
</tr>
<tr class="odd">
<td>public function testPop(array $stack)</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;assertSame('foo', array_pop($stack));</td>
</tr>
<tr class="even">
<td>$this-&gt;assertEmpty($stack);</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>In the example above, the first test, <code>testEmpty()</code>,</td>
</tr>
<tr class="even">
<td>creates a new array and asserts that it is empty. The test then returns</td>
</tr>
<tr class="odd">
<td>the fixture as its result. The second test, <code>testPush()</code>,</td>
</tr>
<tr class="even">
<td>depends on <code>testEmpty()</code> and is passed the result of that</td>
</tr>
<tr class="odd">
<td>depended-upon test as its argument. Finally, <code>testPop()</code></td>
</tr>
<tr class="even">
<td>depends upon <code>testPush()</code>.</td>
</tr>
<tr class="odd">
<td>.. admonition:: Note</td>
</tr>
<tr class="even">
<td>The return value yielded by a producer is passed &quot;as-is&quot; to its</td>
</tr>
<tr class="odd">
<td>consumers by default. This means that when a producer returns an object,</td>
</tr>
<tr class="even">
<td>a reference to that object is passed to the consumers. Instead of</td>
</tr>
<tr class="odd">
<td>a reference either (a) a (deep) copy via <code>@depends clone</code>, or (b) a</td>
</tr>
<tr class="even">
<td>(normal shallow) clone (based on PHP keyword <code>clone</code>) via</td>
</tr>
<tr class="odd">
<td><code>@depends shallowClone</code> are possible too.</td>
</tr>
<tr class="even">
<td>To quickly localize defects, we want our attention to be focussed on</td>
</tr>
<tr class="odd">
<td>relevant failing tests. This is why PHPUnit skips the execution of a test</td>
</tr>
<tr class="even">
<td>when a depended-upon test has failed. This improves defect localization by</td>
</tr>
<tr class="odd">
<td>exploiting the dependencies between tests as shown in</td>
</tr>
<tr class="even">
<td>writing-tests-for-phpunit.examples.DependencyFailureTest.php.</td>
</tr>
<tr class="odd">
<td>.. code-block:: php</td>
</tr>
<tr class="even">
<td>:caption: Exploiting the dependencies between tests</td>
</tr>
<tr class="odd">
<td>:name: writing-tests-for-phpunit.examples.DependencyFailureTest.php</td>
</tr>
<tr class="even">
<td>&lt;?php</td>
</tr>
<tr class="odd">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="even">
<td>class DependencyFailureTest extends TestCase</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>public function testOne()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;assertTrue(false);</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>/**</td>
</tr>
<tr class="odd">
<td>* @depends testOne</td>
</tr>
<tr class="even">
<td>*/</td>
</tr>
<tr class="odd">
<td>public function testTwo()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>.. parsed-literal::</td>
</tr>
<tr class="even">
<td>$ phpunit --verbose DependencyFailureTest</td>
</tr>
<tr class="odd">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="even">
<td>FS</td>
</tr>
<tr class="odd">
<td>Time: 0 seconds, Memory: 5.00Mb</td>
</tr>
<tr class="even">
<td>There was 1 failure:</td>
</tr>
<tr class="odd">
<td>1) DependencyFailureTest::testOne</td>
</tr>
<tr class="even">
<td>Failed asserting that false is true.</td>
</tr>
<tr class="odd">
<td>/home/sb/DependencyFailureTest.php:6</td>
</tr>
<tr class="even">
<td>There was 1 skipped test:</td>
</tr>
<tr class="odd">
<td>1) DependencyFailureTest::testTwo</td>
</tr>
<tr class="even">
<td>This test depends on &quot;DependencyFailureTest::testOne&quot; to pass.</td>
</tr>
<tr class="odd">
<td>FAILURES!</td>
</tr>
<tr class="even">
<td>Tests: 1, Assertions: 1, Failures: 1, Skipped: 1.</td>
</tr>
<tr class="odd">
<td>A test may have more than one <code>@depends</code> annotation.</td>
</tr>
<tr class="even">
<td>PHPUnit does not change the order in which tests are executed, you have to</td>
</tr>
<tr class="odd">
<td>ensure that the dependencies of a test can actually be met before the test</td>
</tr>
<tr class="even">
<td>is run.</td>
</tr>
<tr class="odd">
<td>A test that has more than one <code>@depends</code> annotation</td>
</tr>
<tr class="even">
<td>will get a fixture from the first producer as the first argument, a fixture</td>
</tr>
<tr class="odd">
<td>from the second producer as the second argument, and so on.</td>
</tr>
<tr class="even">
<td>See writing-tests-for-phpunit.examples.MultipleDependencies.php</td>
</tr>
<tr class="odd">
<td>.. code-block:: php</td>
</tr>
<tr class="even">
<td>:caption: Test with multiple dependencies</td>
</tr>
<tr class="odd">
<td>:name: writing-tests-for-phpunit.examples.MultipleDependencies.php</td>
</tr>
<tr class="even">
<td>&lt;?php</td>
</tr>
<tr class="odd">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="even">
<td>class MultipleDependenciesTest extends TestCase</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>public function testProducerFirst()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;assertTrue(true);</td>
</tr>
<tr class="odd">
<td>return 'first';</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>public function testProducerSecond()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;assertTrue(true);</td>
</tr>
<tr class="even">
<td>return 'second';</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>/**</td>
</tr>
<tr class="odd">
<td>* @depends testProducerFirst</td>
</tr>
<tr class="even">
<td>* @depends testProducerSecond</td>
</tr>
<tr class="odd">
<td>*/</td>
</tr>
<tr class="even">
<td>public function testConsumer($a, $b)</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;assertSame('first', $a);</td>
</tr>
<tr class="odd">
<td>$this-&gt;assertSame('second', $b);</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>.. parsed-literal::</td>
</tr>
<tr class="odd">
<td>$ phpunit --verbose MultipleDependenciesTest</td>
</tr>
<tr class="even">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="odd">
<td>...</td>
</tr>
<tr class="even">
<td>Time: 0 seconds, Memory: 3.25Mb</td>
</tr>
<tr class="odd">
<td>OK (3 tests, 4 assertions)</td>
</tr>
<tr class="even">
<td>.. _writing-tests-for-phpunit.data-providers:</td>
</tr>
<tr class="odd">
<td>Data Providers</td>
</tr>
<tr class="even">
<td>##############</td>
</tr>
<tr class="odd">
<td>A test method can accept arbitrary arguments. These arguments are to be</td>
</tr>
<tr class="even">
<td>provided by one or more data provider methods (<code>additionProvider()</code> in</td>
</tr>
<tr class="odd">
<td>writing-tests-for-phpunit.data-providers.examples.DataTest.php).</td>
</tr>
<tr class="even">
<td>The data provider method to be used is specified using the</td>
</tr>
<tr class="odd">
<td><code>@dataProvider</code> annotation.</td>
</tr>
<tr class="even">
<td>A data provider method must be <code>public</code> and either return</td>
</tr>
<tr class="odd">
<td>an array of arrays or an object that implements the <code>Iterator</code></td>
</tr>
<tr class="even">
<td>interface and yields an array for each iteration step. For each array that</td>
</tr>
<tr class="odd">
<td>is part of the collection the test method will be called with the contents</td>
</tr>
<tr class="even">
<td>of the array as its arguments.</td>
</tr>
<tr class="odd">
<td>.. code-block:: php</td>
</tr>
<tr class="even">
<td>:caption: Using a data provider that returns an array of arrays</td>
</tr>
<tr class="odd">
<td>:name: writing-tests-for-phpunit.data-providers.examples.DataTest.php</td>
</tr>
<tr class="even">
<td>&lt;?php</td>
</tr>
<tr class="odd">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="even">
<td>class DataTest extends TestCase</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>/**</td>
</tr>
<tr class="odd">
<td>* @dataProvider additionProvider</td>
</tr>
<tr class="even">
<td>*/</td>
</tr>
<tr class="odd">
<td>public function testAdd($a, $b, $expected)</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;assertSame($expected, $a + $b);</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>public function additionProvider()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>return [</td>
</tr>
<tr class="even">
<td>[0, 0, 0],</td>
</tr>
<tr class="odd">
<td>[0, 1, 1],</td>
</tr>
<tr class="even">
<td>[1, 0, 1],</td>
</tr>
<tr class="odd">
<td>[1, 1, 3]</td>
</tr>
<tr class="even">
<td>];</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>.. parsed-literal::</td>
</tr>
<tr class="even">
<td>$ phpunit DataTest</td>
</tr>
<tr class="odd">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="even">
<td>...F</td>
</tr>
<tr class="odd">
<td>Time: 0 seconds, Memory: 5.75Mb</td>
</tr>
<tr class="even">
<td>There was 1 failure:</td>
</tr>
<tr class="odd">
<td>1) DataTest::testAdd with data set #3 (1, 1, 3)</td>
</tr>
<tr class="even">
<td>Failed asserting that 2 is identical to 3.</td>
</tr>
<tr class="odd">
<td>/home/sb/DataTest.php:9</td>
</tr>
<tr class="even">
<td>FAILURES!</td>
</tr>
<tr class="odd">
<td>Tests: 4, Assertions: 4, Failures: 1.</td>
</tr>
<tr class="even">
<td>When using a large number of datasets it's useful to name each one with string key instead of default numeric.</td>
</tr>
<tr class="odd">
<td>Output will be more verbose as it'll contain that name of a dataset that breaks a test.</td>
</tr>
<tr class="even">
<td>.. code-block:: php</td>
</tr>
<tr class="odd">
<td>:caption: Using a data provider with named datasets</td>
</tr>
<tr class="even">
<td>:name: writing-tests-for-phpunit.data-providers.examples.DataTest1.php</td>
</tr>
<tr class="odd">
<td>&lt;?php</td>
</tr>
<tr class="even">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="odd">
<td>class DataTest extends TestCase</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>/**</td>
</tr>
<tr class="even">
<td>* @dataProvider additionProvider</td>
</tr>
<tr class="odd">
<td>*/</td>
</tr>
<tr class="even">
<td>public function testAdd($a, $b, $expected)</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;assertSame($expected, $a + $b);</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function additionProvider()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>return [</td>
</tr>
<tr class="odd">
<td>'adding zeros' =&gt; [0, 0, 0],</td>
</tr>
<tr class="even">
<td>'zero plus one' =&gt; [0, 1, 1],</td>
</tr>
<tr class="odd">
<td>'one plus zero' =&gt; [1, 0, 1],</td>
</tr>
<tr class="even">
<td>'one plus one' =&gt; [1, 1, 3]</td>
</tr>
<tr class="odd">
<td>];</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>.. parsed-literal::</td>
</tr>
<tr class="odd">
<td>$ phpunit DataTest</td>
</tr>
<tr class="even">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="odd">
<td>...F</td>
</tr>
<tr class="even">
<td>Time: 0 seconds, Memory: 5.75Mb</td>
</tr>
<tr class="odd">
<td>There was 1 failure:</td>
</tr>
<tr class="even">
<td>1) DataTest::testAdd with data set &quot;one plus one&quot; (1, 1, 3)</td>
</tr>
<tr class="odd">
<td>Failed asserting that 2 is identical to 3.</td>
</tr>
<tr class="even">
<td>/home/sb/DataTest.php:9</td>
</tr>
<tr class="odd">
<td>FAILURES!</td>
</tr>
<tr class="even">
<td>Tests: 4, Assertions: 4, Failures: 1.</td>
</tr>
<tr class="odd">
<td>.. code-block:: php</td>
</tr>
<tr class="even">
<td>:caption: Using a data provider that returns an Iterator object</td>
</tr>
<tr class="odd">
<td>:name: writing-tests-for-phpunit.data-providers.examples.DataTest2.php</td>
</tr>
<tr class="even">
<td>&lt;?php</td>
</tr>
<tr class="odd">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="even">
<td>require 'CsvFileIterator.php';</td>
</tr>
<tr class="odd">
<td>class DataTest extends TestCase</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>/**</td>
</tr>
<tr class="even">
<td>* @dataProvider additionProvider</td>
</tr>
<tr class="odd">
<td>*/</td>
</tr>
<tr class="even">
<td>public function testAdd($a, $b, $expected)</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;assertSame($expected, $a + $b);</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function additionProvider()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>return new CsvFileIterator('data.csv');</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>.. parsed-literal::</td>
</tr>
<tr class="even">
<td>$ phpunit DataTest</td>
</tr>
<tr class="odd">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="even">
<td>...F</td>
</tr>
<tr class="odd">
<td>Time: 0 seconds, Memory: 5.75Mb</td>
</tr>
<tr class="even">
<td>There was 1 failure:</td>
</tr>
<tr class="odd">
<td>1) DataTest::testAdd with data set #3 ('1', '1', '3')</td>
</tr>
<tr class="even">
<td>Failed asserting that 2 is identical to 3.</td>
</tr>
<tr class="odd">
<td>/home/sb/DataTest.php:11</td>
</tr>
<tr class="even">
<td>FAILURES!</td>
</tr>
<tr class="odd">
<td>Tests: 4, Assertions: 4, Failures: 1.</td>
</tr>
<tr class="even">
<td>.. code-block:: php</td>
</tr>
<tr class="odd">
<td>:caption: The CsvFileIterator class</td>
</tr>
<tr class="even">
<td>:name: writing-tests-for-phpunit.data-providers.examples.CsvFileIterator.php</td>
</tr>
<tr class="odd">
<td>&lt;?php</td>
</tr>
<tr class="even">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="odd">
<td>class CsvFileIterator implements Iterator</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>protected $file;</td>
</tr>
<tr class="even">
<td>protected $key = 0;</td>
</tr>
<tr class="odd">
<td>protected $current;</td>
</tr>
<tr class="even">
<td>public function __construct($file)</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;file = fopen($file, 'r');</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function __destruct()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>fclose($this-&gt;file);</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function rewind()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>rewind($this-&gt;file);</td>
</tr>
<tr class="odd">
<td>$this-&gt;current = fgetcsv($this-&gt;file);</td>
</tr>
<tr class="even">
<td>$this-&gt;key = 0;</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function valid()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>return !feof($this-&gt;file);</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function key()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>return $this-&gt;key;</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function current()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>return $this-&gt;current;</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function next()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;current = fgetcsv($this-&gt;file);</td>
</tr>
<tr class="odd">
<td>$this-&gt;key++;</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>When a test receives input from both a <code>@dataProvider</code></td>
</tr>
<tr class="odd">
<td>method and from one or more tests it <code>@depends</code> on, the</td>
</tr>
<tr class="even">
<td>arguments from the data provider will come before the ones from</td>
</tr>
<tr class="odd">
<td>depended-upon tests. The arguments from depended-upon tests will be the</td>
</tr>
<tr class="even">
<td>same for each data set.</td>
</tr>
<tr class="odd">
<td>See writing-tests-for-phpunit.data-providers.examples.DependencyAndDataProviderCombo.php</td>
</tr>
<tr class="even">
<td>.. code-block:: php</td>
</tr>
<tr class="odd">
<td>:caption: Combination of @depends and @dataProvider in same test</td>
</tr>
<tr class="even">
<td>:name: writing-tests-for-phpunit.data-providers.examples.DependencyAndDataProviderCombo.php</td>
</tr>
<tr class="odd">
<td>&lt;?php</td>
</tr>
<tr class="even">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="odd">
<td>class DependencyAndDataProviderComboTest extends TestCase</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>public function provider()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>return [['provider1'], ['provider2']];</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>public function testProducerFirst()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;assertTrue(true);</td>
</tr>
<tr class="even">
<td>return 'first';</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function testProducerSecond()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;assertTrue(true);</td>
</tr>
<tr class="odd">
<td>return 'second';</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>/**</td>
</tr>
<tr class="even">
<td>* @depends testProducerFirst</td>
</tr>
<tr class="odd">
<td>* @depends testProducerSecond</td>
</tr>
<tr class="even">
<td>* @dataProvider provider</td>
</tr>
<tr class="odd">
<td>*/</td>
</tr>
<tr class="even">
<td>public function testConsumer()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;assertSame(</td>
</tr>
<tr class="odd">
<td>['provider1', 'first', 'second'],</td>
</tr>
<tr class="even">
<td>func_get_args()</td>
</tr>
<tr class="odd">
<td>);</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>.. parsed-literal::</td>
</tr>
<tr class="odd">
<td>$ phpunit --verbose DependencyAndDataProviderComboTest</td>
</tr>
<tr class="even">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="odd">
<td>...F</td>
</tr>
<tr class="even">
<td>Time: 0 seconds, Memory: 3.50Mb</td>
</tr>
<tr class="odd">
<td>There was 1 failure:</td>
</tr>
<tr class="even">
<td>1) DependencyAndDataProviderComboTest::testConsumer with data set #1 ('provider2')</td>
</tr>
<tr class="odd">
<td>Failed asserting that two arrays are identical.</td>
</tr>
<tr class="even">
<td>--- Expected</td>
</tr>
<tr class="odd">
<td>+++ Actual</td>
</tr>
<tr class="even">
<td>@@ @@</td>
</tr>
<tr class="odd">
<td>Array &amp;0 (</td>
</tr>
<tr class="even">
<td>- 0 =&gt; 'provider1'</td>
</tr>
<tr class="odd">
<td>+ 0 =&gt; 'provider2'</td>
</tr>
<tr class="even">
<td>1 =&gt; 'first'</td>
</tr>
<tr class="odd">
<td>2 =&gt; 'second'</td>
</tr>
<tr class="even">
<td>)</td>
</tr>
<tr class="odd">
<td>/home/sb/DependencyAndDataProviderComboTest.php:32</td>
</tr>
<tr class="even">
<td>FAILURES!</td>
</tr>
<tr class="odd">
<td>Tests: 4, Assertions: 4, Failures: 1.</td>
</tr>
<tr class="even">
<td>.. code-block:: php</td>
</tr>
<tr class="odd">
<td>:caption: Using multiple data providers for a single test</td>
</tr>
<tr class="even">
<td>:name: writing-tests-for-phpunit.data-providers.examples2.DataTest.php</td>
</tr>
<tr class="odd">
<td>&lt;?php</td>
</tr>
<tr class="even">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="odd">
<td>class DataTest extends TestCase</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>/**</td>
</tr>
<tr class="even">
<td>* @dataProvider additionWithNonNegativeNumbersProvider</td>
</tr>
<tr class="odd">
<td>* @dataProvider additionWithNegativeNumbersProvider</td>
</tr>
<tr class="even">
<td>*/</td>
</tr>
<tr class="odd">
<td>public function testAdd($a, $b, $expected)</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;assertSame($expected, $a + $b);</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>public function additionWithNonNegativeNumbersProvider()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>return [</td>
</tr>
<tr class="even">
<td>[0, 1, 1],</td>
</tr>
<tr class="odd">
<td>[1, 0, 1],</td>
</tr>
<tr class="even">
<td>[1, 1, 3]</td>
</tr>
<tr class="odd">
<td>];</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>public function additionWithNegativeNumbersProvider()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>return [</td>
</tr>
<tr class="even">
<td>[-1, 1, 0],</td>
</tr>
<tr class="odd">
<td>[-1, -1, -2],</td>
</tr>
<tr class="even">
<td>[1, -1, 0]</td>
</tr>
<tr class="odd">
<td>];</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>.. parsed-literal::</td>
</tr>
<tr class="odd">
<td>$ phpunit DataTest</td>
</tr>
<tr class="even">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="odd">
<td>..F... 6 / 6 (100%)</td>
</tr>
<tr class="even">
<td>Time: 0 seconds, Memory: 5.75Mb</td>
</tr>
<tr class="odd">
<td>There was 1 failure:</td>
</tr>
<tr class="even">
<td>1) DataTest::testAdd with data set #3 (1, 1, 3)</td>
</tr>
<tr class="odd">
<td>Failed asserting that 2 is identical to 3.</td>
</tr>
<tr class="even">
<td>/home/sb/DataTest.php:12</td>
</tr>
<tr class="odd">
<td>FAILURES!</td>
</tr>
<tr class="even">
<td>Tests: 6, Assertions: 6, Failures: 1.</td>
</tr>
<tr class="odd">
<td>.. admonition:: Note</td>
</tr>
<tr class="even">
<td>When a test depends on a test that uses data providers, the depending</td>
</tr>
<tr class="odd">
<td>test will be executed when the test it depends upon is successful for at</td>
</tr>
<tr class="even">
<td>least one data set. The result of a test that uses data providers cannot</td>
</tr>
<tr class="odd">
<td>be injected into a depending test.</td>
</tr>
<tr class="even">
<td>.. admonition:: Note</td>
</tr>
<tr class="odd">
<td>All data providers are executed before both the call to the <code>setUpBeforeClass()</code></td>
</tr>
<tr class="even">
<td>static method and the first call to the <code>setUp()</code> method.</td>
</tr>
<tr class="odd">
<td>Because of that you can't access any variables you create there from</td>
</tr>
<tr class="even">
<td>within a data provider. This is required in order for PHPUnit to be able</td>
</tr>
<tr class="odd">
<td>to compute the total number of tests.</td>
</tr>
<tr class="even">
<td>.. _writing-tests-for-phpunit.exceptions:</td>
</tr>
<tr class="odd">
<td>Testing Exceptions</td>
</tr>
<tr class="even">
<td>##################</td>
</tr>
<tr class="odd">
<td>writing-tests-for-phpunit.exceptions.examples.ExceptionTest.php</td>
</tr>
<tr class="even">
<td>shows how to use the <code>expectException()</code> method to test</td>
</tr>
<tr class="odd">
<td>whether an exception is thrown by the code under test.</td>
</tr>
<tr class="even">
<td>.. code-block:: php</td>
</tr>
<tr class="odd">
<td>:caption: Using the expectException() method</td>
</tr>
<tr class="even">
<td>:name: writing-tests-for-phpunit.exceptions.examples.ExceptionTest.php</td>
</tr>
<tr class="odd">
<td>&lt;?php</td>
</tr>
<tr class="even">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="odd">
<td>class ExceptionTest extends TestCase</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>public function testException()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;expectException(InvalidArgumentException::class);</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>.. parsed-literal::</td>
</tr>
<tr class="odd">
<td>$ phpunit ExceptionTest</td>
</tr>
<tr class="even">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="odd">
<td>F</td>
</tr>
<tr class="even">
<td>Time: 0 seconds, Memory: 4.75Mb</td>
</tr>
<tr class="odd">
<td>There was 1 failure:</td>
</tr>
<tr class="even">
<td>1) ExceptionTest::testException</td>
</tr>
<tr class="odd">
<td>Failed asserting that exception of type &quot;InvalidArgumentException&quot; is thrown.</td>
</tr>
<tr class="even">
<td>FAILURES!</td>
</tr>
<tr class="odd">
<td>Tests: 1, Assertions: 1, Failures: 1.</td>
</tr>
<tr class="even">
<td>In addition to the <code>expectException()</code> method the</td>
</tr>
<tr class="odd">
<td><code>expectExceptionCode()</code>,</td>
</tr>
<tr class="even">
<td><code>expectExceptionMessage()</code>, and</td>
</tr>
<tr class="odd">
<td><code>expectExceptionMessageMatches()</code> methods exist to set up</td>
</tr>
<tr class="even">
<td>expectations for exceptions raised by the code under test.</td>
</tr>
<tr class="odd">
<td>.. admonition:: Note</td>
</tr>
<tr class="even">
<td>Note that <code>expectExceptionMessage()</code> asserts that the <code>$actual</code></td>
</tr>
<tr class="odd">
<td>message contains the <code>$expected</code> message and does not perform</td>
</tr>
<tr class="even">
<td>an exact string comparison.</td>
</tr>
<tr class="odd">
<td>.. _writing-tests-for-phpunit.errors:</td>
</tr>
<tr class="even">
<td>Testing PHP Errors, Warnings, and Notices</td>
</tr>
<tr class="odd">
<td>#########################################</td>
</tr>
<tr class="even">
<td>By default, PHPUnit converts PHP errors, warnings, and notices that are</td>
</tr>
<tr class="odd">
<td>triggered during the execution of a test to an exception. Among other benefits,</td>
</tr>
<tr class="even">
<td>this makes it possible to expect that a PHP error, warning, or notice is</td>
</tr>
<tr class="odd">
<td>triggered in a test as shown in</td>
</tr>
<tr class="even">
<td>writing-tests-for-phpunit.exceptions.examples.ErrorTest.php.</td>
</tr>
<tr class="odd">
<td>.. admonition:: Note</td>
</tr>
<tr class="even">
<td>PHP's <code>error_reporting</code> runtime configuration can</td>
</tr>
<tr class="odd">
<td>limit which errors PHPUnit will convert to exceptions. If you are</td>
</tr>
<tr class="even">
<td>having issues with this feature, be sure PHP is not configured to</td>
</tr>
<tr class="odd">
<td>suppress the type of error you are interested in.</td>
</tr>
<tr class="even">
<td>.. code-block:: php</td>
</tr>
<tr class="odd">
<td>:caption: Expecting PHP errors, warnings, and notices</td>
</tr>
<tr class="even">
<td>:name: writing-tests-for-phpunit.exceptions.examples.ErrorTest.php</td>
</tr>
<tr class="odd">
<td>&lt;?php declare(strict_types=1);</td>
</tr>
<tr class="even">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="odd">
<td>final class ErrorTest extends TestCase</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>public function testDeprecationCanBeExpected(): void</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;expectDeprecation();</td>
</tr>
<tr class="even">
<td>// Optionally test that the message is equal to a string</td>
</tr>
<tr class="odd">
<td>$this-&gt;expectDeprecationMessage('foo');</td>
</tr>
<tr class="even">
<td>// Or optionally test that the message matches a regular expression</td>
</tr>
<tr class="odd">
<td>$this-&gt;expectDeprecationMessageMatches('/foo/');</td>
</tr>
<tr class="even">
<td>trigger_error('foo', E_USER_DEPRECATED);</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function testNoticeCanBeExpected(): void</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;expectNotice();</td>
</tr>
<tr class="odd">
<td>// Optionally test that the message is equal to a string</td>
</tr>
<tr class="even">
<td>$this-&gt;expectNoticeMessage('foo');</td>
</tr>
<tr class="odd">
<td>// Or optionally test that the message matches a regular expression</td>
</tr>
<tr class="even">
<td>$this-&gt;expectNoticeMessageMatches('/foo/');</td>
</tr>
<tr class="odd">
<td>trigger_error('foo', E_USER_NOTICE);</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>public function testWarningCanBeExpected(): void</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;expectWarning();</td>
</tr>
<tr class="even">
<td>// Optionally test that the message is equal to a string</td>
</tr>
<tr class="odd">
<td>$this-&gt;expectWarningMessage('foo');</td>
</tr>
<tr class="even">
<td>// Or optionally test that the message matches a regular expression</td>
</tr>
<tr class="odd">
<td>$this-&gt;expectWarningMessageMatches('/foo/');</td>
</tr>
<tr class="even">
<td>trigger_error('foo', E_USER_WARNING);</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function testErrorCanBeExpected(): void</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;expectError();</td>
</tr>
<tr class="odd">
<td>// Optionally test that the message is equal to a string</td>
</tr>
<tr class="even">
<td>$this-&gt;expectErrorMessage('foo');</td>
</tr>
<tr class="odd">
<td>// Or optionally test that the message matches a regular expression</td>
</tr>
<tr class="even">
<td>$this-&gt;expectErrorMessageMatches('/foo/');</td>
</tr>
<tr class="odd">
<td>trigger_error('foo', E_USER_ERROR);</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>When testing code that uses PHP built-in functions such as <code>fopen()</code> that</td>
</tr>
<tr class="odd">
<td>may trigger errors it can sometimes be useful to use error suppression while</td>
</tr>
<tr class="even">
<td>testing. This allows you to check the return values by suppressing notices</td>
</tr>
<tr class="odd">
<td>that would lead to an exception raised by PHPUnit's error handler.</td>
</tr>
<tr class="even">
<td>.. code-block:: php</td>
</tr>
<tr class="odd">
<td>:caption: Testing return values of code that uses PHP Errors</td>
</tr>
<tr class="even">
<td>:name: writing-tests-for-phpunit.exceptions.examples.TriggerErrorReturnValue.php</td>
</tr>
<tr class="odd">
<td>&lt;?php</td>
</tr>
<tr class="even">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="odd">
<td>class ErrorSuppressionTest extends TestCase</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>public function testFileWriting()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$writer = new FileWriter;</td>
</tr>
<tr class="even">
<td>$this-&gt;assertFalse(@$writer-&gt;write('/is-not-writeable/file', 'stuff'));</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>class FileWriter</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>public function write($file, $content)</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$file = fopen($file, 'w');</td>
</tr>
<tr class="even">
<td>if ($file == false) {</td>
</tr>
<tr class="odd">
<td>return false;</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>// ...</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>.. parsed-literal::</td>
</tr>
<tr class="odd">
<td>$ phpunit ErrorSuppressionTest</td>
</tr>
<tr class="even">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="odd">
<td>.</td>
</tr>
<tr class="even">
<td>Time: 1 seconds, Memory: 5.25Mb</td>
</tr>
<tr class="odd">
<td>OK (1 test, 1 assertion)</td>
</tr>
<tr class="even">
<td>Without the error suppression the test would fail reporting</td>
</tr>
<tr class="odd">
<td><code>fopen(/is-not-writeable/file): failed to open stream: No such file or directory</code>.</td>
</tr>
<tr class="even">
<td>.. _writing-tests-for-phpunit.output:</td>
</tr>
<tr class="odd">
<td>Testing Output</td>
</tr>
<tr class="even">
<td>##############</td>
</tr>
<tr class="odd">
<td>Sometimes you want to assert that the execution of a method, for</td>
</tr>
<tr class="even">
<td>instance, generates an expected output (via <code>echo</code> or</td>
</tr>
<tr class="odd">
<td><code>print</code>, for example). The</td>
</tr>
<tr class="even">
<td><code>PHPUnit\Framework\TestCase</code> class uses PHP's</td>
</tr>
<tr class="odd">
<td>`Output</td>
</tr>
<tr class="even">
<td>Buffering &lt;<a href="http://www.php.net/manual/en/ref.outcontrol.php" class="uri">http://www.php.net/manual/en/ref.outcontrol.php</a>&gt;`_ feature to provide the functionality that is</td>
</tr>
<tr class="odd">
<td>necessary for this.</td>
</tr>
<tr class="even">
<td>writing-tests-for-phpunit.output.examples.OutputTest.php</td>
</tr>
<tr class="odd">
<td>shows how to use the <code>expectOutputString()</code> method to</td>
</tr>
<tr class="even">
<td>set the expected output. If this expected output is not generated, the</td>
</tr>
<tr class="odd">
<td>test will be counted as a failure.</td>
</tr>
<tr class="even">
<td>.. code-block:: php</td>
</tr>
<tr class="odd">
<td>:caption: Testing the output of a function or method</td>
</tr>
<tr class="even">
<td>:name: writing-tests-for-phpunit.output.examples.OutputTest.php</td>
</tr>
<tr class="odd">
<td>&lt;?php</td>
</tr>
<tr class="even">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="odd">
<td>class OutputTest extends TestCase</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>public function testExpectFooActualFoo()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;expectOutputString('foo');</td>
</tr>
<tr class="even">
<td>print 'foo';</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>public function testExpectBarActualBaz()</td>
</tr>
<tr class="odd">
<td>{</td>
</tr>
<tr class="even">
<td>$this-&gt;expectOutputString('bar');</td>
</tr>
<tr class="odd">
<td>print 'baz';</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>.. parsed-literal::</td>
</tr>
<tr class="odd">
<td>$ phpunit OutputTest</td>
</tr>
<tr class="even">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="odd">
<td>.F</td>
</tr>
<tr class="even">
<td>Time: 0 seconds, Memory: 5.75Mb</td>
</tr>
<tr class="odd">
<td>There was 1 failure:</td>
</tr>
<tr class="even">
<td>1) OutputTest::testExpectBarActualBaz</td>
</tr>
<tr class="odd">
<td>Failed asserting that two strings are equal.</td>
</tr>
<tr class="even">
<td>--- Expected</td>
</tr>
<tr class="odd">
<td>+++ Actual</td>
</tr>
<tr class="even">
<td>@@ @@</td>
</tr>
<tr class="odd">
<td>-'bar'</td>
</tr>
<tr class="even">
<td>+'baz'</td>
</tr>
<tr class="odd">
<td>FAILURES!</td>
</tr>
<tr class="even">
<td>Tests: 2, Assertions: 2, Failures: 1.</td>
</tr>
<tr class="odd">
<td>writing-tests-for-phpunit.output.tables.api</td>
</tr>
<tr class="even">
<td>shows the methods provided for testing output</td>
</tr>
<tr class="odd">
<td>.. rst-class:: table</td>
</tr>
<tr class="even">
<td>.. list-table:: Methods for testing output</td>
</tr>
<tr class="odd">
<td>:name: writing-tests-for-phpunit.output.tables.api</td>
</tr>
<tr class="even">
<td>:header-rows: 1</td>
</tr>
<tr class="odd">
<td>* - Method</td>
</tr>
<tr class="even">
<td>- Meaning</td>
</tr>
<tr class="odd">
<td>* - <code>void expectOutputRegex(string $regularExpression)</code></td>
</tr>
<tr class="even">
<td>- Set up the expectation that the output matches a <code>$regularExpression</code>.</td>
</tr>
<tr class="odd">
<td>* - <code>void expectOutputString(string $expectedString)</code></td>
</tr>
<tr class="even">
<td>- Set up the expectation that the output is equal to an <code>$expectedString</code>.</td>
</tr>
<tr class="odd">
<td>* - <code>bool setOutputCallback(callable $callback)</code></td>
</tr>
<tr class="even">
<td>- Sets up a callback that is used to, for instance, normalize the actual output.</td>
</tr>
<tr class="odd">
<td>* - <code>string getActualOutput()</code></td>
</tr>
<tr class="even">
<td>- Get the actual output.</td>
</tr>
<tr class="odd">
<td>.. admonition:: Note</td>
</tr>
<tr class="even">
<td>A test that emits output will fail in strict mode.</td>
</tr>
<tr class="odd">
<td>.. _writing-tests-for-phpunit.error-output:</td>
</tr>
<tr class="even">
<td>Error output</td>
</tr>
<tr class="odd">
<td>############</td>
</tr>
<tr class="even">
<td>Whenever a test fails PHPUnit tries its best to provide you with as much</td>
</tr>
<tr class="odd">
<td>context as possible that can help to identify the problem.</td>
</tr>
<tr class="even">
<td>.. code-block:: php</td>
</tr>
<tr class="odd">
<td>:caption: Error output generated when an array comparison fails</td>
</tr>
<tr class="even">
<td>:name: writing-tests-for-phpunit.error-output.examples.ArrayDiffTest.php</td>
</tr>
<tr class="odd">
<td>&lt;?php</td>
</tr>
<tr class="even">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="odd">
<td>class ArrayDiffTest extends TestCase</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>public function testEquality()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;assertSame(</td>
</tr>
<tr class="even">
<td>[1, 2, 3, 4, 5, 6],</td>
</tr>
<tr class="odd">
<td>[1, 2, 33, 4, 5, 6]</td>
</tr>
<tr class="even">
<td>);</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>.. parsed-literal::</td>
</tr>
<tr class="even">
<td>$ phpunit ArrayDiffTest</td>
</tr>
<tr class="odd">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="even">
<td>F</td>
</tr>
<tr class="odd">
<td>Time: 0 seconds, Memory: 5.25Mb</td>
</tr>
<tr class="even">
<td>There was 1 failure:</td>
</tr>
<tr class="odd">
<td>1) ArrayDiffTest::testEquality</td>
</tr>
<tr class="even">
<td>Failed asserting that two arrays are identical.</td>
</tr>
<tr class="odd">
<td>--- Expected</td>
</tr>
<tr class="even">
<td>+++ Actual</td>
</tr>
<tr class="odd">
<td>@@ @@</td>
</tr>
<tr class="even">
<td>Array (</td>
</tr>
<tr class="odd">
<td>0 =&gt; 1</td>
</tr>
<tr class="even">
<td>1 =&gt; 2</td>
</tr>
<tr class="odd">
<td>- 2 =&gt; 3</td>
</tr>
<tr class="even">
<td>+ 2 =&gt; 33</td>
</tr>
<tr class="odd">
<td>3 =&gt; 4</td>
</tr>
<tr class="even">
<td>4 =&gt; 5</td>
</tr>
<tr class="odd">
<td>5 =&gt; 6</td>
</tr>
<tr class="even">
<td>)</td>
</tr>
<tr class="odd">
<td>/home/sb/ArrayDiffTest.php:7</td>
</tr>
<tr class="even">
<td>FAILURES!</td>
</tr>
<tr class="odd">
<td>Tests: 1, Assertions: 1, Failures: 1.</td>
</tr>
<tr class="even">
<td>In this example only one of the array values differs and the other values</td>
</tr>
<tr class="odd">
<td>are shown to provide context on where the error occurred.</td>
</tr>
<tr class="even">
<td>When the generated output would be long to read PHPUnit will split it up</td>
</tr>
<tr class="odd">
<td>and provide a few lines of context around every difference.</td>
</tr>
<tr class="even">
<td>.. code-block:: php</td>
</tr>
<tr class="odd">
<td>:caption: Error output when an array comparison of an long array fails</td>
</tr>
<tr class="even">
<td>:name: writing-tests-for-phpunit.error-output.examples.LongArrayDiffTest.php</td>
</tr>
<tr class="odd">
<td>&lt;?php</td>
</tr>
<tr class="even">
<td>use PHPUnitFrameworkTestCase;</td>
</tr>
<tr class="odd">
<td>class LongArrayDiffTest extends TestCase</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>public function testEquality()</td>
</tr>
<tr class="even">
<td>{</td>
</tr>
<tr class="odd">
<td>$this-&gt;assertSame(</td>
</tr>
<tr class="even">
<td>[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 2, 3, 4, 5, 6],</td>
</tr>
<tr class="odd">
<td>[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 2, 33, 4, 5, 6]</td>
</tr>
<tr class="even">
<td>);</td>
</tr>
<tr class="odd">
<td>}</td>
</tr>
<tr class="even">
<td>}</td>
</tr>
<tr class="odd">
<td>.. parsed-literal::</td>
</tr>
<tr class="even">
<td>$ phpunit LongArrayDiffTest</td>
</tr>
<tr class="odd">
<td>PHPUnit .0 by Sebastian Bergmann and contributors.</td>
</tr>
<tr class="even">
<td>F</td>
</tr>
<tr class="odd">
<td>Time: 0 seconds, Memory: 5.25Mb</td>
</tr>
<tr class="even">
<td>There was 1 failure:</td>
</tr>
<tr class="odd">
<td>1) LongArrayDiffTest::testEquality</td>
</tr>
<tr class="even">
<td>Failed asserting that two arrays are identical.</td>
</tr>
<tr class="odd">
<td>--- Expected</td>
</tr>
<tr class="even">
<td>+++ Actual</td>
</tr>
<tr class="odd">
<td>@@ @@</td>
</tr>
<tr class="even">
<td>11 =&gt; 0</td>
</tr>
<tr class="odd">
<td>12 =&gt; 1</td>
</tr>
<tr class="even">
<td>13 =&gt; 2</td>
</tr>
<tr class="odd">
<td>- 14 =&gt; 3</td>
</tr>
<tr class="even">
<td>+ 14 =&gt; 33</td>
</tr>
<tr class="odd">
<td>15 =&gt; 4</td>
</tr>
<tr class="even">
<td>16 =&gt; 5</td>
</tr>
<tr class="odd">
<td>17 =&gt; 6</td>
</tr>
<tr class="even">
<td>)</td>
</tr>
<tr class="odd">
<td>/home/sb/LongArrayDiffTest.php:7</td>
</tr>
<tr class="even">
<td>FAILURES!</td>
</tr>
<tr class="odd">
<td>Tests: 1, Assertions: 1, Failures: 1.</td>
</tr>
<tr class="even">
<td>.. _writing-tests-for-phpunit.error-output.edge-cases:</td>
</tr>
<tr class="odd">
<td>Edge cases</td>
</tr>
</tbody>
</table>

When a comparison fails PHPUnit creates textual representations of the
input values and compares those. Due to that implementation a diff might
show more problems than actually exist.

This only happens when using `assertEquals()` or other 'weak' comparison
functions on arrays or objects.

    <?php
    use PHPUnit\Framework\TestCase;

    class ArrayWeakComparisonTest extends TestCase
    {
        public function testEquality()
        {
            $this->assertEquals(
                [1, 2, 3, 4, 5, 6],
                ['1', 2, 33, 4, 5, 6]
            );
        }
    }

In this example the difference in the first index between `1` and `'1'`
is reported even though `assertEquals()` considers the values as a
match.
