# webservco/coding-standards

A collection of coding standards and configuration files.

Custom, opinionated coding standards based on [PSR12](https://www.php-fig.org/psr/psr-12/), [SlevomatCodingStandard](https://github.com/slevomat/coding-standard), and [PHPCompatibility](https://github.com/PHPCompatibility/PHPCompatibility).

---

## Setup

```shell
composer require --dev webservco/coding-standards @dev
```

Optionally install any of the dependencies from `require-dev` that you wish to use in yout project.

---

## Usage

## [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)

Example configuration file `.phpcs/php-coding-standard.xml`:

```xml
<?xml version="1.0"?>
<ruleset name="WebServCo-CodingStandard-PHP81">
	<description>Custom, opinionated coding standards based on PSR12, SlevomatCodingStandard, and PHPCompatibility.</description>
    <rule ref="vendor/webservco/coding-standards/phpcs/ruleset-psr-php81-slevomat.xml">
        <properties>
			<property name="rootNamespaces" type="array">
				<element key="src/Project" value="Project" />
                <element key="tests/unit" value="Tests" />
			</property>
		</properties>
    </rule>
</ruleset>
```

---

## [PHPUnit](https://phpunit.de/)

Since it is not possible to configure the working directory in PHPUnit, a default configuration file can not be provided.

An example configuration file can be found in the component skeleton project.

---
