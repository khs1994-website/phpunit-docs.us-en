Assertions
==========

This appendix lists the various assertion methods that are available.

Static vs. Non-Static Usage of Assertion Methods
------------------------------------------------

PHPUnit's assertions are implemented in `PHPUnit\Framework\Assert`.
`PHPUnit\Framework\TestCase` inherits from `PHPUnit\Framework\Assert`.

The assertion methods are declared static and can be invoked from any
context using `PHPUnit\Framework\Assert::assertTrue()`, for instance, or
using `$this->assertTrue()` or `self::assertTrue()`, for instance, in a
class that extends `PHPUnit\Framework\TestCase`.

In fact, you can even use global function wrappers such as
`assertTrue()` in any context (including classes that extend
`PHPUnit\Framework\TestCase`) when you (manually) include the
src/Framework/Assert/Functions.php sourcecode file that comes with
PHPUnit.

A common question, especially from developers new to PHPUnit, is whether
using `$this->assertTrue()` or `self::assertTrue()`, for instance, is
"the right way" to invoke an assertion. The short answer is: there is no
right way. And there is no wrong way, either. It is a matter of personal
preference.

For most people it just "feels right" to use `$this->assertTrue()`
because the test method is invoked on a test object. The fact that the
assertion methods are declared static allows for (re)using them outside
the scope of a test object. Lastly, the global function wrappers allow
developers to type less characters (`assertTrue()` instead of
`$this->assertTrue()` or `self::assertTrue()`).

assertArrayHasKey()
-------------------

`assertArrayHasKey(mixed $key, array $array[, string $message = ''])`

Reports an error identified by `$message` if `$array` does not have the
`$key`.

`assertArrayNotHasKey()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class ArrayHasKeyTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertArrayHasKey('foo', ['bar' => 'baz']);
        }
    }

assertClassHasAttribute()
-------------------------

`assertClassHasAttribute(string $attributeName, string $className[, string $message = ''])`

Reports an error identified by `$message` if `$className::attributeName`
does not exist.

`assertClassNotHasAttribute()` is the inverse of this assertion and
takes the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class ClassHasAttributeTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertClassHasAttribute('foo', stdClass::class);
        }
    }

assertClassHasStaticAttribute()
-------------------------------

`assertClassHasStaticAttribute(string $attributeName, string $className[, string $message = ''])`

Reports an error identified by `$message` if `$className::attributeName`
does not exist.

`assertClassNotHasStaticAttribute()` is the inverse of this assertion
and takes the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class ClassHasStaticAttributeTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertClassHasStaticAttribute('foo', stdClass::class);
        }
    }

assertContains()
----------------

`assertContains(mixed $needle, iterable $haystack[, string $message = ''])`

Reports an error identified by `$message` if `$needle` is not an element
of `$haystack`.

`assertNotContains()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class ContainsTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertContains(4, [1, 2, 3]);
        }
    }

assertStringContainsString()
----------------------------

`assertStringContainsString(string $needle, string $haystack[, string $message = ''])`

Reports an error identified by `$message` if `$needle` is not a
substring of `$haystack`.

`assertStringNotContainsString()` is the inverse of this assertion and
takes the same arguments.

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class StringContainsStringTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertStringContainsString('foo', 'bar');
        }
    }

assertStringContainsStringIgnoringCase()
----------------------------------------

`assertStringContainsStringIgnoringCase(string $needle, string $haystack[, string $message = ''])`

Reports an error identified by `$message` if `$needle` is not a
substring of `$haystack`.

Differences in casing are ignored when `$needle` is searched for in
`$haystack`.

`assertStringNotContainsStringIgnoringCase()` is the inverse of this
assertion and takes the same arguments.

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class StringContainsStringIgnoringCaseTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertStringContainsStringIgnoringCase('foo', 'bar');
        }
    }

assertContainsOnly()
--------------------

`assertContainsOnly(string $type, iterable $haystack[, boolean $isNativeType = null, string $message = ''])`

Reports an error identified by `$message` if `$haystack` does not
contain only variables of type `$type`.

`$isNativeType` is a flag used to indicate whether `$type` is a native
PHP type or not.

`assertNotContainsOnly()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class ContainsOnlyTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertContainsOnly('string', ['1', '2', 3]);
        }
    }

assertContainsOnlyInstancesOf()
-------------------------------

`assertContainsOnlyInstancesOf(string $classname, Traversable|array $haystack[, string $message = ''])`

Reports an error identified by `$message` if `$haystack` does not
contain only instances of class `$classname`.

    <?php
    use PHPUnit\Framework\TestCase;

    class ContainsOnlyInstancesOfTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertContainsOnlyInstancesOf(
                Foo::class,
                [new Foo, new Bar, new Foo]
            );
        }
    }

assertCount()
-------------

`assertCount($expectedCount, $haystack[, string $message = ''])`

Reports an error identified by `$message` if the number of elements in
`$haystack` is not `$expectedCount`.

`assertNotCount()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class CountTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertCount(0, ['foo']);
        }
    }

assertDirectoryExists()
-----------------------

`assertDirectoryExists(string $directory[, string $message = ''])`

Reports an error identified by `$message` if the directory specified by
`$directory` does not exist.

`assertDirectoryNotExists()` is the inverse of this assertion and takes
the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class DirectoryExistsTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertDirectoryExists('/path/to/directory');
        }
    }

assertDirectoryIsReadable()
---------------------------

`assertDirectoryIsReadable(string $directory[, string $message = ''])`

Reports an error identified by `$message` if the directory specified by
`$directory` is not a directory or is not readable.

`assertDirectoryNotIsReadable()` is the inverse of this assertion and
takes the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class DirectoryIsReadableTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertDirectoryIsReadable('/path/to/directory');
        }
    }

assertDirectoryIsWritable()
---------------------------

`assertDirectoryIsWritable(string $directory[, string $message = ''])`

Reports an error identified by `$message` if the directory specified by
`$directory` is not a directory or is not writable.

`assertDirectoryNotIsWritable()` is the inverse of this assertion and
takes the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class DirectoryIsWritableTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertDirectoryIsWritable('/path/to/directory');
        }
    }

assertEmpty()
-------------

`assertEmpty(mixed $actual[, string $message = ''])`

Reports an error identified by `$message` if `$actual` is not empty.

`assertNotEmpty()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class EmptyTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertEmpty(['foo']);
        }
    }

assertEqualXMLStructure()
-------------------------

`assertEqualXMLStructure(DOMElement $expectedElement, DOMElement $actualElement[, boolean $checkAttributes = false, string $message = ''])`

Reports an error identified by `$message` if the XML Structure of the
DOMElement in `$actualElement` is not equal to the XML structure of the
DOMElement in `$expectedElement`.

    <?php
    use PHPUnit\Framework\TestCase;

    class EqualXMLStructureTest extends TestCase
    {
        public function testFailureWithDifferentNodeNames()
        {
            $expected = new DOMElement('foo');
            $actual = new DOMElement('bar');

            $this->assertEqualXMLStructure($expected, $actual);
        }

        public function testFailureWithDifferentNodeAttributes()
        {
            $expected = new DOMDocument;
            $expected->loadXML('<foo bar="true" />');

            $actual = new DOMDocument;
            $actual->loadXML('<foo/>');

            $this->assertEqualXMLStructure(
              $expected->firstChild, $actual->firstChild, true
            );
        }

        public function testFailureWithDifferentChildrenCount()
        {
            $expected = new DOMDocument;
            $expected->loadXML('<foo><bar/><bar/><bar/></foo>');

            $actual = new DOMDocument;
            $actual->loadXML('<foo><bar/></foo>');

            $this->assertEqualXMLStructure(
              $expected->firstChild, $actual->firstChild
            );
        }

        public function testFailureWithDifferentChildren()
        {
            $expected = new DOMDocument;
            $expected->loadXML('<foo><bar/><bar/><bar/></foo>');

            $actual = new DOMDocument;
            $actual->loadXML('<foo><baz/><baz/><baz/></foo>');

            $this->assertEqualXMLStructure(
              $expected->firstChild, $actual->firstChild
            );
        }
    }

assertEquals()
--------------

`assertEquals(mixed $expected, mixed $actual[, string $message = ''])`

Reports an error identified by `$message` if the two variables
`$expected` and `$actual` are not equal.

`assertNotEquals()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class EqualsTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertEquals(1, 0);
        }

        public function testFailure2()
        {
            $this->assertEquals('bar', 'baz');
        }

        public function testFailure3()
        {
            $this->assertEquals("foo\nbar\nbaz\n", "foo\nbah\nbaz\n");
        }
    }

More specialized comparisons are used for specific argument types for
`$expected` and `$actual`, see below.

`assertEquals(DOMDocument $expected, DOMDocument $actual[, string $message = ''])`

Reports an error identified by `$message` if the uncommented canonical
form of the XML documents represented by the two DOMDocument objects
`$expected` and `$actual` are not equal.

    <?php
    use PHPUnit\Framework\TestCase;

    class EqualsTest extends TestCase
    {
        public function testFailure()
        {
            $expected = new DOMDocument;
            $expected->loadXML('<foo><bar/></foo>');

            $actual = new DOMDocument;
            $actual->loadXML('<bar><foo/></bar>');

            $this->assertEquals($expected, $actual);
        }
    }

`assertEquals(object $expected, object $actual[, string $message = ''])`

Reports an error identified by `$message` if the two objects `$expected`
and `$actual` do not have equal attribute values.

    <?php
    use PHPUnit\Framework\TestCase;

    class EqualsTest extends TestCase
    {
        public function testFailure()
        {
            $expected = new stdClass;
            $expected->foo = 'foo';
            $expected->bar = 'bar';

            $actual = new stdClass;
            $actual->foo = 'bar';
            $actual->baz = 'bar';

            $this->assertEquals($expected, $actual);
        }
    }

`assertEquals(array $expected, array $actual[, string $message = ''])`

Reports an error identified by `$message` if the two arrays `$expected`
and `$actual` are not equal.

    <?php
    use PHPUnit\Framework\TestCase;

    class EqualsTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertEquals(['a', 'b', 'c'], ['a', 'c', 'd']);
        }
    }

assertEqualsCanonicalizing()
----------------------------

`assertEqualsCanonicalizing(mixed $expected, mixed $actual[, string $message = ''])`

Reports an error identified by `$message` if the two variables
`$expected` and `$actual` are not equal.

The contents of `$expected` and `$actual` are canonicalized before they
are compared. For instance, when the two variables `$expected` and
`$actual` are arrays, then these arrays are sorted before they are
compared. When `$expected` and `$actual` are objects, each object is
converted to an array containing all private, protected and public
attributes.

`assertNotEqualsCanonicalizing()` is the inverse of this assertion and
takes the same arguments.

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class EqualsCanonicalizingTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertEqualsCanonicalizing([3, 2, 1], [2, 3, 0, 1]);
        }
    }

assertEqualsIgnoringCase()
--------------------------

`assertEqualsIgnoringCase(mixed $expected, mixed $actual[, string $message = ''])`

Reports an error identified by `$message` if the two variables
`$expected` and `$actual` are not equal.

Differences in casing are ignored for the comparison of `$expected` and
`$actual`.

`assertNotEqualsIgnoringCase()` is the inverse of this assertion and
takes the same arguments.

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class EqualsIgnoringCaseTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertEqualsIgnoringCase('foo', 'BAR');
        }
    }

assertEqualsWithDelta()
-----------------------

`assertEqualsWithDelta(mixed $expected, mixed $actual, float $delta[, string $message = ''])`

Reports an error identified by `$message` if the absolute difference
between `$expected` and `$actual` is greater than `$delta`.

Please read "[What Every Computer Scientist Should Know About
Floating-Point
Arithmetic](http://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html)"
to understand why `$delta` is necessary.

`assertNotEqualsWithDelta()` is the inverse of this assertion and takes
the same arguments.

    <?php declare(strict_types=1);
    use PHPUnit\Framework\TestCase;

    final class EqualsWithDeltaTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertEqualsWithDelta(1.0, 1.5, 0.1);
        }
    }

assertFalse()
-------------

`assertFalse(bool $condition[, string $message = ''])`

Reports an error identified by `$message` if `$condition` is `true`.

`assertNotFalse()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class FalseTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertFalse(true);
        }
    }

assertFileEquals()
------------------

`assertFileEquals(string $expected, string $actual[, string $message = ''])`

Reports an error identified by `$message` if the file specified by
`$expected` does not have the same contents as the file specified by
`$actual`.

`assertFileNotEquals()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class FileEqualsTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertFileEquals('/home/sb/expected', '/home/sb/actual');
        }
    }

assertFileExists()
------------------

`assertFileExists(string $filename[, string $message = ''])`

Reports an error identified by `$message` if the file specified by
`$filename` does not exist.

`assertFileNotExists()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class FileExistsTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertFileExists('/path/to/file');
        }
    }

assertFileIsReadable()
----------------------

`assertFileIsReadable(string $filename[, string $message = ''])`

Reports an error identified by `$message` if the file specified by
`$filename` is not a file or is not readable.

`assertFileNotIsReadable()` is the inverse of this assertion and takes
the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class FileIsReadableTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertFileIsReadable('/path/to/file');
        }
    }

assertFileIsWritable()
----------------------

`assertFileIsWritable(string $filename[, string $message = ''])`

Reports an error identified by `$message` if the file specified by
`$filename` is not a file or is not writable.

`assertFileNotIsWritable()` is the inverse of this assertion and takes
the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class FileIsWritableTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertFileIsWritable('/path/to/file');
        }
    }

assertGreaterThan()
-------------------

`assertGreaterThan(mixed $expected, mixed $actual[, string $message = ''])`

Reports an error identified by `$message` if the value of `$actual` is
not greater than the value of `$expected`.

    <?php
    use PHPUnit\Framework\TestCase;

    class GreaterThanTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertGreaterThan(2, 1);
        }
    }

assertGreaterThanOrEqual()
--------------------------

`assertGreaterThanOrEqual(mixed $expected, mixed $actual[, string $message = ''])`

Reports an error identified by `$message` if the value of `$actual` is
not greater than or equal to the value of `$expected`.

    <?php
    use PHPUnit\Framework\TestCase;

    class GreatThanOrEqualTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertGreaterThanOrEqual(2, 1);
        }
    }
    ?>

assertInfinite()
----------------

`assertInfinite(mixed $variable[, string $message = ''])`

Reports an error identified by `$message` if `$variable` is not `INF`.

`assertFinite()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class InfiniteTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertInfinite(1);
        }
    }
    ?>

assertInstanceOf()
------------------

`assertInstanceOf($expected, $actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not an
instance of `$expected`.

`assertNotInstanceOf()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class InstanceOfTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertInstanceOf(RuntimeException::class, new Exception);
        }
    }
    ?>

assertIsArray()
---------------

`assertIsArray($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`array`.

`assertIsNotArray()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class ArrayTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsArray(null);
        }
    }

    $ phpunit ArrayTest
    PHPUnit |version|.0 by Sebastian Bergmann and contributors.

    F

    Time: 0 seconds, Memory: 5.00Mb

    There was 1 failure:

    1) ArrayTest::testFailure
    Failed asserting that null is of type "array".

    /home/sb/ArrayTest.php:8

    FAILURES!
    Tests: 1, Assertions: 1, Failures: 1.

assertIsBool()
--------------

`assertIsBool($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`bool`.

`assertIsNotBool()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class BoolTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsBool(null);
        }
    }

    $ phpunit BoolTest
    PHPUnit |version|.0 by Sebastian Bergmann and contributors.

    F

    Time: 0 seconds, Memory: 5.00Mb

    There was 1 failure:

    1) BoolTest::testFailure
    Failed asserting that null is of type "bool".

    /home/sb/BoolTest.php:8

    FAILURES!
    Tests: 1, Assertions: 1, Failures: 1.

assertIsCallable()
------------------

`assertIsCallable($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`callable`.

`assertIsNotCallable()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class CallableTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsCallable(null);
        }
    }

    $ phpunit CallableTest
    PHPUnit |version|.0 by Sebastian Bergmann and contributors.

    F

    Time: 0 seconds, Memory: 5.00Mb

    There was 1 failure:

    1) CallableTest::testFailure
    Failed asserting that null is of type "callable".

    /home/sb/CallableTest.php:8

    FAILURES!
    Tests: 1, Assertions: 1, Failures: 1.

assertIsFloat()
---------------

`assertIsFloat($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`float`.

`assertIsNotFloat()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class FloatTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsFloat(null);
        }
    }

    $ phpunit FloatTest
    PHPUnit |version|.0 by Sebastian Bergmann and contributors.

    F

    Time: 0 seconds, Memory: 5.00Mb

    There was 1 failure:

    1) FloatTest::testFailure
    Failed asserting that null is of type "float".

    /home/sb/FloatTest.php:8

    FAILURES!
    Tests: 1, Assertions: 1, Failures: 1.

assertIsInt()
-------------

`assertIsInt($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`int`.

`assertIsNotInt()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class IntTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsInt(null);
        }
    }

    $ phpunit IntTest
    PHPUnit |version|.0 by Sebastian Bergmann and contributors.

    F

    Time: 0 seconds, Memory: 5.00Mb

    There was 1 failure:

    1) IntTest::testFailure
    Failed asserting that null is of type "int".

    /home/sb/IntTest.php:8

    FAILURES!
    Tests: 1, Assertions: 1, Failures: 1.

assertIsIterable()
------------------

`assertIsIterable($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`iterable`.

`assertIsNotIterable()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class IterableTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsIterable(null);
        }
    }

    $ phpunit IterableTest
    PHPUnit |version|.0 by Sebastian Bergmann and contributors.

    F

    Time: 0 seconds, Memory: 5.00Mb

    There was 1 failure:

    1) IterableTest::testFailure
    Failed asserting that null is of type "iterable".

    /home/sb/IterableTest.php:8

    FAILURES!
    Tests: 1, Assertions: 1, Failures: 1.

assertIsNumeric()
-----------------

`assertIsNumeric($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`numeric`.

`assertIsNotNumeric()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class NumericTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsNumeric(null);
        }
    }

    $ phpunit NumericTest
    PHPUnit |version|.0 by Sebastian Bergmann and contributors.

    F

    Time: 0 seconds, Memory: 5.00Mb

    There was 1 failure:

    1) NumericTest::testFailure
    Failed asserting that null is of type "numeric".

    /home/sb/NumericTest.php:8

    FAILURES!
    Tests: 1, Assertions: 1, Failures: 1.

assertIsObject()
----------------

`assertIsObject($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`object`.

`assertIsNotObject()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class ObjectTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsObject(null);
        }
    }

    $ phpunit ObjectTest
    PHPUnit |version|.0 by Sebastian Bergmann and contributors.

    F

    Time: 0 seconds, Memory: 5.00Mb

    There was 1 failure:

    1) ObjectTest::testFailure
    Failed asserting that null is of type "object".

    /home/sb/ObjectTest.php:8

    FAILURES!
    Tests: 1, Assertions: 1, Failures: 1.

assertIsResource()
------------------

`assertIsResource($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`resource`.

`assertIsNotResource()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class ResourceTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsResource(null);
        }
    }

    $ phpunit ResourceTest
    PHPUnit |version|.0 by Sebastian Bergmann and contributors.

    F

    Time: 0 seconds, Memory: 5.00Mb

    There was 1 failure:

    1) ResourceTest::testFailure
    Failed asserting that null is of type "resource".

    /home/sb/ResourceTest.php:8

    FAILURES!
    Tests: 1, Assertions: 1, Failures: 1.

assertIsScalar()
----------------

`assertIsScalar($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`scalar`.

`assertIsNotScalar()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class ScalarTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsScalar(null);
        }
    }

    $ phpunit ScalarTest
    PHPUnit |version|.0 by Sebastian Bergmann and contributors.

    F

    Time: 0 seconds, Memory: 5.00Mb

    There was 1 failure:

    1) ScalarTest::testFailure
    Failed asserting that null is of type "scalar".

    /home/sb/ScalarTest.php:8

    FAILURES!
    Tests: 1, Assertions: 1, Failures: 1.

assertIsString()
----------------

`assertIsString($actual[, $message = ''])`

Reports an error identified by `$message` if `$actual` is not of type
`string`.

`assertIsNotString()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class StringTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsString(null);
        }
    }

assertIsReadable()
------------------

`assertIsReadable(string $filename[, string $message = ''])`

Reports an error identified by `$message` if the file or directory
specified by `$filename` is not readable.

`assertNotIsReadable()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class IsReadableTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsReadable('/path/to/unreadable');
        }
    }
    ?>

assertIsWritable()
------------------

`assertIsWritable(string $filename[, string $message = ''])`

Reports an error identified by `$message` if the file or directory
specified by `$filename` is not writable.

`assertNotIsWritable()` is the inverse of this assertion and takes the
same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class IsWritableTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertIsWritable('/path/to/unwritable');
        }
    }
    ?>

assertJsonFileEqualsJsonFile()
------------------------------

`assertJsonFileEqualsJsonFile(mixed $expectedFile, mixed $actualFile[, string $message = ''])`

Reports an error identified by `$message` if the value of `$actualFile`
does not match the value of `$expectedFile`.

    <?php
    use PHPUnit\Framework\TestCase;

    class JsonFileEqualsJsonFileTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertJsonFileEqualsJsonFile(
              'path/to/fixture/file', 'path/to/actual/file');
        }
    }
    ?>

assertJsonStringEqualsJsonFile()
--------------------------------

`assertJsonStringEqualsJsonFile(mixed $expectedFile, mixed $actualJson[, string $message = ''])`

Reports an error identified by `$message` if the value of `$actualJson`
does not match the value of `$expectedFile`.

    <?php
    use PHPUnit\Framework\TestCase;

    class JsonStringEqualsJsonFileTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertJsonStringEqualsJsonFile(
                'path/to/fixture/file', json_encode(['Mascot' => 'ux'])
            );
        }
    }
    ?>

assertJsonStringEqualsJsonString()
----------------------------------

`assertJsonStringEqualsJsonString(mixed $expectedJson, mixed $actualJson[, string $message = ''])`

Reports an error identified by `$message` if the value of `$actualJson`
does not match the value of `$expectedJson`.

    <?php
    use PHPUnit\Framework\TestCase;

    class JsonStringEqualsJsonStringTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertJsonStringEqualsJsonString(
                json_encode(['Mascot' => 'Tux']),
                json_encode(['Mascot' => 'ux'])
            );
        }
    }
    ?>

assertLessThan()
----------------

`assertLessThan(mixed $expected, mixed $actual[, string $message = ''])`

Reports an error identified by `$message` if the value of `$actual` is
not less than the value of `$expected`.

    <?php
    use PHPUnit\Framework\TestCase;

    class LessThanTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertLessThan(1, 2);
        }
    }
    ?>

assertLessThanOrEqual()
-----------------------

`assertLessThanOrEqual(mixed $expected, mixed $actual[, string $message = ''])`

Reports an error identified by `$message` if the value of `$actual` is
not less than or equal to the value of `$expected`.

    <?php
    use PHPUnit\Framework\TestCase;

    class LessThanOrEqualTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertLessThanOrEqual(1, 2);
        }
    }
    ?>

assertNan()
-----------

`assertNan(mixed $variable[, string $message = ''])`

Reports an error identified by `$message` if `$variable` is not `NAN`.

    <?php
    use PHPUnit\Framework\TestCase;

    class NanTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertNan(1);
        }
    }
    ?>

assertNull()
------------

`assertNull(mixed $variable[, string $message = ''])`

Reports an error identified by `$message` if `$variable` is not `null`.

`assertNotNull()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class NullTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertNull('foo');
        }
    }
    ?>

assertObjectHasAttribute()
--------------------------

`assertObjectHasAttribute(string $attributeName, object $object[, string $message = ''])`

Reports an error identified by `$message` if `$object->attributeName`
does not exist.

`assertObjectNotHasAttribute()` is the inverse of this assertion and
takes the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class ObjectHasAttributeTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertObjectHasAttribute('foo', new stdClass);
        }
    }
    ?>

assertRegExp()
--------------

`assertRegExp(string $pattern, string $string[, string $message = ''])`

Reports an error identified by `$message` if `$string` does not match
the regular expression `$pattern`.

`assertNotRegExp()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class RegExpTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertRegExp('/foo/', 'bar');
        }
    }
    ?>

assertStringMatchesFormat()
---------------------------

`assertStringMatchesFormat(string $format, string $string[, string $message = ''])`

Reports an error identified by `$message` if the `$string` does not
match the `$format` string.

`assertStringNotMatchesFormat()` is the inverse of this assertion and
takes the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class StringMatchesFormatTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertStringMatchesFormat('%i', 'foo');
        }
    }
    ?>

The format string may contain the following placeholders:

-

> `%e`: Represents a directory separator, for example `/` on Linux.

-

> `%s`: One or more of anything (character or white space) except the
> end of line character.

-

> `%S`: Zero or more of anything (character or white space) except the
> end of line character.

-

> `%a`: One or more of anything (character or white space) including the
> end of line character.

-

> `%A`: Zero or more of anything (character or white space) including
> the end of line character.

-

> `%w`: Zero or more white space characters.

-

> `%i`: A signed integer value, for example `+3142`, `-3142`.

-

> `%d`: An unsigned integer value, for example `123456`.

-

> `%x`: One or more hexadecimal character. That is, characters in the
> range `0-9`, `a-f`, `A-F`.

-

> `%f`: A floating point number, for example: `3.142`, `-3.142`,
> `3.142E-10`, `3.142e+10`.

-

> `%c`: A single character of any sort.

-

> `%%`: A literal percent character: `%`.

assertStringMatchesFormatFile()
-------------------------------

`assertStringMatchesFormatFile(string $formatFile, string $string[, string $message = ''])`

Reports an error identified by `$message` if the `$string` does not
match the contents of the `$formatFile`.

`assertStringNotMatchesFormatFile()` is the inverse of this assertion
and takes the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class StringMatchesFormatFileTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertStringMatchesFormatFile('/path/to/expected.txt', 'foo');
        }
    }
    ?>

assertSame()
------------

`assertSame(mixed $expected, mixed $actual[, string $message = ''])`

Reports an error identified by `$message` if the two variables
`$expected` and `$actual` do not have the same type and value.

`assertNotSame()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class SameTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertSame('2204', 2204);
        }
    }
    ?>

`assertSame(object $expected, object $actual[, string $message = ''])`

Reports an error identified by `$message` if the two variables
`$expected` and `$actual` do not reference the same object.

    <?php
    use PHPUnit\Framework\TestCase;

    class SameTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertSame(new stdClass, new stdClass);
        }
    }
    ?>

assertStringEndsWith()
----------------------

`assertStringEndsWith(string $suffix, string $string[, string $message = ''])`

Reports an error identified by `$message` if the `$string` does not end
with `$suffix`.

`assertStringEndsNotWith()` is the inverse of this assertion and takes
the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class StringEndsWithTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertStringEndsWith('suffix', 'foo');
        }
    }
    ?>

assertStringEqualsFile()
------------------------

`assertStringEqualsFile(string $expectedFile, string $actualString[, string $message = ''])`

Reports an error identified by `$message` if the file specified by
`$expectedFile` does not have `$actualString` as its contents.

`assertStringNotEqualsFile()` is the inverse of this assertion and takes
the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class StringEqualsFileTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertStringEqualsFile('/home/sb/expected', 'actual');
        }
    }
    ?>

assertStringStartsWith()
------------------------

`assertStringStartsWith(string $prefix, string $string[, string $message = ''])`

Reports an error identified by `$message` if the `$string` does not
start with `$prefix`.

`assertStringStartsNotWith()` is the inverse of this assertion and takes
the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class StringStartsWithTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertStringStartsWith('prefix', 'foo');
        }
    }
    ?>

assertThat()
------------

More complex assertions can be formulated using the
`PHPUnit\Framework\Constraint` classes. They can be evaluated using the
`assertThat()` method. appendixes.assertions.assertThat.example shows
how the `logicalNot()` and `equalTo()` constraints can be used to
express the same assertion as `assertNotEquals()`.

`assertThat(mixed $value, PHPUnit\Framework\Constraint $constraint[, $message = ''])`

Reports an error identified by `$message` if the `$value` does not match
the `$constraint`.

    <?php
    use PHPUnit\Framework\TestCase;

    class BiscuitTest extends TestCase
    {
        public function testEquals()
        {
            $theBiscuit = new Biscuit('Ginger');
            $myBiscuit  = new Biscuit('Ginger');

            $this->assertThat(
              $theBiscuit,
              $this->logicalNot(
                $this->equalTo($myBiscuit)
              )
            );
        }
    }
    ?>

appendixes.assertions.assertThat.tables.constraints shows the available
`PHPUnit\Framework\Constraint` classes.

assertTrue()
------------

`assertTrue(bool $condition[, string $message = ''])`

Reports an error identified by `$message` if `$condition` is `false`.

`assertNotTrue()` is the inverse of this assertion and takes the same
arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class TrueTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertTrue(false);
        }
    }
    ?>

assertXmlFileEqualsXmlFile()
----------------------------

`assertXmlFileEqualsXmlFile(string $expectedFile, string $actualFile[, string $message = ''])`

Reports an error identified by `$message` if the XML document in
`$actualFile` is not equal to the XML document in `$expectedFile`.

`assertXmlFileNotEqualsXmlFile()` is the inverse of this assertion and
takes the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class XmlFileEqualsXmlFileTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertXmlFileEqualsXmlFile(
              '/home/sb/expected.xml', '/home/sb/actual.xml');
        }
    }
    ?>

assertXmlStringEqualsXmlFile()
------------------------------

`assertXmlStringEqualsXmlFile(string $expectedFile, string $actualXml[, string $message = ''])`

Reports an error identified by `$message` if the XML document in
`$actualXml` is not equal to the XML document in `$expectedFile`.

`assertXmlStringNotEqualsXmlFile()` is the inverse of this assertion and
takes the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class XmlStringEqualsXmlFileTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertXmlStringEqualsXmlFile(
              '/home/sb/expected.xml', '<foo><baz/></foo>');
        }
    }
    ?>

assertXmlStringEqualsXmlString()
--------------------------------

`assertXmlStringEqualsXmlString(string $expectedXml, string $actualXml[, string $message = ''])`

Reports an error identified by `$message` if the XML document in
`$actualXml` is not equal to the XML document in `$expectedXml`.

`assertXmlStringNotEqualsXmlString()` is the inverse of this assertion
and takes the same arguments.

    <?php
    use PHPUnit\Framework\TestCase;

    class XmlStringEqualsXmlStringTest extends TestCase
    {
        public function testFailure()
        {
            $this->assertXmlStringEqualsXmlString(
              '<foo><bar/></foo>', '<foo><baz/></foo>');
        }
    }
    ?>
