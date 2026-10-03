# webservco/coding-standards

A collection of coding standards and configuration files.

Custom, opinionated coding standards based on [PSR12](https://www.php-fig.org/psr/psr-12/) and [SlevomatCodingStandard](https://github.com/slevomat/coding-standard).

---

## Setup

```shell
composer require --dev webservco/coding-standards
```

Optionally, install any of the dependencies from `require-dev` that you wish to use in your project.

---

## Upgrading from v1

- PHP_CodeSniffer:
	- use `phpcs/ruleset-psr.xml` or `phpcs/ruleset-psr-slevomat.xml`; `ruleset-psr-php83*.xml` and `ruleset-psr-php84*.xml` still work but are deprecated; the PHP 7.4, 8.1 and 8.2 rulesets were removed;
	- PHPCompatibility is no longer included: remove `phpcompatibility/php-compatibility` from your project (and any `PHPCompatibility` / `testVersion` lines from your own ruleset), then require `"squizlabs/php_codesniffer": "^4"` to get current Slevomat versions;
	- newer Slevomat versions add sniffs (e.g. `SlevomatCodingStandard.Classes.ReadonlyClass`), so expect new errors.
- PHPUnit: only `phpunit/phpunit-12.xml` and `phpunit/phpunit-13.xml` are provided; the `--display-*` options are now set in the configuration files and can be removed from your scripts.
- PHPStan: stricter checks are enabled (uninitialized properties, callable signatures, possibly nonexistent array offsets, too wide return types, benevolent union types), so expect new errors.
- Phan: requires Phan 6; `UnusedSuppressionPlugin` is enabled.

---

## Components

## [Phan](https://github.com/phan/phan)

Usage:

```shell
vendor/bin/phan --config-file vendor/webservco/coding-standards/phan/config.php
```

---

## [PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer)

Example configuration file `.phpcs/php-coding-standard.xml`, to be placed in own project:

```xml
<?xml version="1.0"?>
<ruleset name="Project-CodingStandard">
	<description>Custom, opinionated coding standards based on PSR12 and SlevomatCodingStandard.</description>
    <rule ref="vendor/webservco/coding-standards/phpcs/ruleset-psr-slevomat.xml">
        <properties>
			<property name="rootNamespaces" type="array">
				<element key="src/Project" value="Project" />
                <element key="tests/unit" value="Tests" />
			</property>
		</properties>
    </rule>
</ruleset>
```

Usage:

```shell
vendor/bin/phpcs --standard=.phpcs/php-coding-standard.xml --extensions=php -sp bin config public resources src tests
```

Rulesets:

- `phpcs/ruleset-psr.xml`: PSR-12;
- `phpcs/ruleset-psr-slevomat.xml`: PSR-12, Slevomat;
- `phpcs/ruleset-namespaces.xml`: Slevomat namespace usage.

Deprecated, kept for backwards compatibility (they only include the unversioned rulesets above):

- `phpcs/ruleset-psr-php83.xml`, `phpcs/ruleset-psr-php84.xml`: use `phpcs/ruleset-psr.xml`;
- `phpcs/ruleset-psr-php83-slevomat.xml`, `phpcs/ruleset-psr-php84-slevomat.xml`: use `phpcs/ruleset-psr-slevomat.xml`.

---

## [PHPMD](https://github.com/phpmd/phpmd)

Usage:

```shell
vendor/bin/phpmd bin,config,public,resources,src,tests json vendor/webservco/coding-standards/phpmd/phpmd-rule-set.xml
```

---

## [PHPStan](https://github.com/phpstan/phpstan)

Symfony support
- install symfony related packages:
	- (if using Doctrine) "phpstan/phpstan-doctrine": "^2",
	- "phpstan/phpstan-symfony": "^2",
- (if using Doctrine) create `.phpstan/get_doctrine_manager.php`, as in phpstan-doctrine documentation
- use specific `phpstan-symfony.neon` or `phpstan-symfony-doctrine.neon` configuration files

Usage:

```shell
vendor/bin/phpstan analyse bin config public resources src tests --ansi -c vendor/webservco/coding-standards/phpstan/phpstan.neon
```

---

## [PHPUnit](https://phpunit.de/)

Composer scripts example:

```json
{
	"scripts": {
		"test" : "XDEBUG_MODE=coverage vendor/bin/phpunit --colors=always --configuration vendor/webservco/coding-standards/phpunit/phpunit-13.xml",
        "test:dox" : "@test --testdox"
	}
}
```

Usage:

```shell
ddev xdebug on
clear && ddev exec XDEBUG_MODE=coverage composer test:dox
```
---

## [Psalm](https://github.com/vimeo/psalm)

Usage:

```shell
vendor/bin/psalm --config=vendor/webservco/coding-standards/psalm/psalm.xml --no-diff
```

---
