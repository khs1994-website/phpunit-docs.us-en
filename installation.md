Installing PHPUnit
==================

Requirements
------------

PHPUnit requires PHP 7.3; using the latest version of PHP is highly
recommended.

PHPUnit requires the [dom](http://php.net/manual/en/dom.setup.php) and
[json](http://php.net/manual/en/json.installation.php) extensions, which
are normally enabled by default.

PHPUnit also requires the
[pcre](http://php.net/manual/en/pcre.installation.php),
[reflection](http://php.net/manual/en/reflection.installation.php), and
[spl](http://php.net/manual/en/spl.installation.php) extensions. These
standard extensions are enabled by default and cannot be disabled
without patching PHP's build system and/or C sources.

The code coverage report feature requires the
[Xdebug](http://xdebug.org/) (2.7.0 or later) and
[tokenizer](http://php.net/manual/en/tokenizer.installation.php)
extensions. Generating XML reports requires the
[xmlwriter](http://php.net/manual/en/xmlwriter.installation.php)
extension.

PHP Archive (PHAR)
------------------

The easiest way to obtain PHPUnit is to download a [PHP Archive
(PHAR)](http://php.net/phar) that has all required (as well as some
optional) dependencies of PHPUnit bundled in a single file.

The [phar](http://php.net/manual/en/phar.installation.php) extension is
required for using PHP Archives (PHAR).

If the [Suhosin](http://suhosin.org/) extension is enabled, you need to
allow execution of PHARs in your `php.ini`:

    suhosin.executor.include.whitelist = phar

The PHPUnit PHAR can be used immediately after download:

It is a common practice to make the PHAR executable:

### Verifying PHPUnit PHAR Releases

All official releases of code distributed by the PHPUnit Project are
signed by the release manager for the release. PGP signatures and SHA256
hashes are available for verification on
[phar.phpunit.de](https://phar.phpunit.de/).

The following example details how release verification works. We start
by downloading phpunit.phar as well as its detached PGP signature
phpunit.phar.asc:

We want to verify PHPUnit's PHP Archive (phpunit-x.y.phar) against its
detached signature (phpunit-x.y.phar.asc):

We don't have the release manager's public key (`6372C20A`) in our local
system. In order to proceed with the verification we need to retrieve
the release manager's public key from a key server. One such server is
pgp.uni-mainz.de. The public key servers are linked together, so you
should be able to connect to any key server.

Now we have received a public key for an entity known as "Sebastian
Bergmann &lt;<sb@sebastian-bergmann.de>&gt;". However, we have no way of
verifying this key was created by the person known as Sebastian
Bergmann. But, let's try to verify the release signature again.

At this point, the signature is good, but we don't trust this key. A
good signature means that the file has not been tampered. However, due
to the nature of public key cryptography, you need to additionally
verify that key `6372C20A` was created by the real Sebastian Bergmann.

Any attacker can create a public key and upload it to the public key
servers. They can then create a malicious release signed by this fake
key. Then, if you tried to verify the signature of this corrupt release,
it would succeed because the key was not the "real" key. Therefore, you
need to validate the authenticity of this key. Validating the
authenticity of a public key, however, is outside the scope of this
documentation.

Manually verifying the authenticity and integrity of a PHPUnit PHAR
using GPG is tedious. This is why PHIVE, the PHAR Installation and
Verification Environment, was created. You can learn about PHIVE on its
[website](https://phar.io/)

Composer
--------

Simply add a (development-time) dependency on `phpunit/phpunit` to your
project's `composer.json` file if you use
[Composer](https://getcomposer.org/) to manage the dependencies of your
project:

Global Installation
-------------------

Please note that it is not recommended to install PHPUnit globally, as
`/usr/bin/phpunit` or `/usr/local/bin/phpunit`, for instance.

Instead, PHPUnit should be managed as a project-local dependency.

Either put the PHAR of the specific PHPUnit version you need in your
project's `tools` directory (which should be managed by PHIVE) or depend
on the specific PHPUnit version you need in your project's
`composer.json` if you use Composer.

Webserver
---------

PHPUnit is a framework for writing as well as a commandline tool for
running tests. Writing and running tests is a development-time activity.
There is no reason why PHPUnit should be installed on a webserver.

**If you upload PHPUnit to a webserver then your deployment process is
broken. On a more general note, if your** `vendor` **directory is
publicly accessible on your webserver then your deployment process is
also broken.**

Please note that if you upload PHPUnit to a webserver "bad things" may
happen. [You have been
warned.](https://thephp.cc/news/2020/02/phpunit-a-security-risk)
