# webservco/coding-standards

A collection of coding standards and configuration files.

Custom, opinionated coding standards based on [PSR12](https://www.php-fig.org/psr/psr-12/), [SlevomatCodingStandard](https://github.com/slevomat/coding-standard), and [PHPCompatibility](https://github.com/PHPCompatibility/PHPCompatibility).

---

## Setup

```shell
composer require --dev webservco/coding-standards
```

Optionally, install any of the dependencies from `require-dev` that you wish to use in your project.

---

## Components

## [Phan](https://github.com/phan/phan)

Usage:

```shell
vendor/bin/phan --config-file vendor/webservco/coding-standards/phan/config.php
```

---

## [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)

Example configuration file `.phpcs/php-coding-standard.xml`, to be placed in own project:

```xml
<?xml version="1.0"?>
<ruleset name="WebServCo-CodingStandard-PHP82">
	<description>Custom, opinionated coding standards based on PSR12, SlevomatCodingStandard, and PHPCompatibility.</description>
    <rule ref="vendor/webservco/coding-standards/phpcs/ruleset-psr-php82-slevomat.xml">
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

- `phpcs/ruleset-namespaces.xml`: Slevomat namespace usage;
- `phpcs/ruleset-psr-php74-slevomat.xml`: PHP 7.4, PSR-12, Slevomat;
- `phpcs/ruleset-psr-php81.xml`: PHP 8.1, PSR-12;
- `phpcs/ruleset-psr-php81-slevomat.xml`: PHP 8.1, PSR-12, Slevomat;
- `phpcs/ruleset-psr-php82.xml`: PHP 8.2, PSR-12;
- `phpcs/ruleset-psr-php82-slevomat.xml`: PHP 8.2, PSR-12, Slevomat;

---

## [PHPMD](https://github.com/phpmd/phpmd)

Usage:

```shell
vendor/bin/phpmd bin,config,public,resources,src,tests json vendor/webservco/coding-standards/phpmd/phpmd-rule-set.xml

```

---

## [PHPStan](https://github.com/phpstan/phpstan)

Usage:

```shell
vendor/bin/phpstan analyse bin config public resources src tests --ansi -c vendor/webservco/coding-standards/phpstan/phpstan.neon --level=max
```

---

## [PHPUnit](https://phpunit.de/)

Composer scripts example:

```json
{
	"scripts": {
		"test" : "vendor/bin/phpunit --colors=always --configuration vendor/webservco/coding-standards/phpunit/phpunit-10.xml --display-deprecations --display-errors --display-incomplete --display-notices --display-skipped --display-warnings",
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
