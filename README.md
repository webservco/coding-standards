# webservco/coding-standards

Custom, opinionated coding standards based on [PSR12](https://www.php-fig.org/psr/psr-12/), [SlevomatCodingStandard](https://github.com/slevomat/coding-standard), and [PHPCompatibility](https://github.com/PHPCompatibility/PHPCompatibility).

---

## Usage

```shell
composer require --dev webservco/coding-standards @dev
```

### Example configuration file

```xml
<?xml version="1.0"?>
<ruleset name="WebServCo-CodingStandard-PHP81">
	<description>Custom, opinionated coding standards based on PSR12, SlevomatCodingStandard, and PHPCompatibility.</description>
    <rule ref="vendor/webservco/coding-standards/ruleset-php81.xml">
        <properties>
			<property name="rootNamespaces" type="array">
				<element key="src/Project" value="Project" />
                <element key="tests/unit" value="Tests" />
			</property>
		</properties>
    </rule>
</ruleset>
```
