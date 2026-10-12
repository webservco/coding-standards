# webservco/coding-standards

A collection of coding standards and configuration files.

Shared, opinionated configuration for PHP_CodeSniffer, PHPStan, Psalm, Phan, PHPMD and PHPUnit, based on [PSR-12](https://www.php-fig.org/psr/psr-12/) and [Slevomat Coding Standard](https://github.com/slevomat/coding-standard).

- Requires: PHP 8.4+
- Related: `webservco/component-common` (the components' phpcs ruleset, built on this one)

---

## Installation

```shell
composer require --dev webservco/coding-standards
```

The package only holds configuration files, so require each tool you use yourself. `require-dev` in [composer.json](composer.json) lists the supported versions.

---

## Usage

Run the tools from the project root, with paths pointing into `vendor/webservco/coding-standards/`. Every config expects the [PDS skeleton](https://github.com/php-pds/skeleton) directories (`bin config public resources src tests`).

| Tool | Config | Command |
| --- | --- | --- |
| [Phan](https://github.com/phan/phan) 6 | `phan/config.php` | `vendor/bin/phan --config-file vendor/webservco/coding-standards/phan/config.php` |
| [PHPMD](https://github.com/phpmd/phpmd) 3 | `phpmd/phpmd-rule-set.xml` | `vendor/bin/phpmd analyze --no-progress --format json --ruleset vendor/webservco/coding-standards/phpmd/phpmd-rule-set.xml bin config public resources src tests` |
| [PHPStan](https://github.com/phpstan/phpstan) 2 | `phpstan/phpstan.neon` | `vendor/bin/phpstan analyse bin config public resources src tests --ansi -c vendor/webservco/coding-standards/phpstan/phpstan.neon` |
| [PHPUnit](https://phpunit.de/) 12, 13 | `phpunit/phpunit-13.xml` | `XDEBUG_MODE=coverage vendor/bin/phpunit --colors=always --configuration vendor/webservco/coding-standards/phpunit/phpunit-13.xml` |
| [Psalm](https://github.com/vimeo/psalm) 6 | `psalm/psalm.xml` | `vendor/bin/psalm --config=vendor/webservco/coding-standards/psalm/psalm.xml --no-diff` |

### PHP_CodeSniffer

Create `.phpcs/php-coding-standard.xml` in the project:

```xml
<?xml version="1.0"?>
<ruleset name="Project-CodingStandard">
    <rule ref="vendor/webservco/coding-standards/phpcs/ruleset-psr-slevomat.xml">
    </rule>
    <rule ref="SlevomatCodingStandard.Files.TypeNameMatchesFileName">
        <properties>
            <property name="rootNamespaces" type="array">
                <element key="src/Project" value="Project" />
                <element key="tests/Unit" value="Tests\Unit" />
            </property>
        </properties>
    </rule>
</ruleset>
```

```shell
vendor/bin/phpcs --standard=.phpcs/php-coding-standard.xml --extensions=php -sp bin config public resources src tests
```

The rulesets are `phpcs/ruleset-psr.xml` (PSR-12), `phpcs/ruleset-psr-slevomat.xml` (PSR-12 and Slevomat) and `phpcs/ruleset-namespaces.xml` (Slevomat namespace and `use` rules only).

### Symfony

PHPStan and Psalm have `-symfony` and `-symfony-doctrine` variants of their configs. Require `phpstan/phpstan-symfony` and `psalm/plugin-symfony`, and for Doctrine also `phpstan/phpstan-doctrine` and `weirdan/doctrine-psalm-plugin`. The Doctrine PHPStan config also needs `.phpstan/get_doctrine_manager.php`, as described in the phpstan-doctrine documentation.

---

## Notes

PHPStan runs at level max, with extra checks for uninitialized properties, callable signatures, possibly nonexistent array offsets, too wide return types and benevolent union types.

The PHPMD rule set uses rules that only exist in PHPMD 3, and `composer.json` conflicts with older versions. Separate paths with spaces. To suppress a warning, use the `#[\PHPMD\Attribute\SuppressWarnings(...)]` attribute. The `@SuppressWarnings` annotation is deprecated, and the `UnusedSuppression` rule doesn't check it.

The PHPUnit configs set the `--display-*` options and write coverage to `var/tmp/phpunit/`. With ddev, run `ddev xdebug on` first.

### Upgrading from v1

- PHP_CodeSniffer: replace `phpcs/ruleset-psr-phpXY.xml` with `phpcs/ruleset-psr.xml`, and `phpcs/ruleset-psr-phpXY-slevomat.xml` with `phpcs/ruleset-psr-slevomat.xml`. PHPCompatibility is gone: remove `phpcompatibility/php-compatibility` and any `PHPCompatibility` or `testVersion` lines, then require `"squizlabs/php_codesniffer": "^4"`. Newer Slevomat versions add sniffs such as `SlevomatCodingStandard.Classes.ReadonlyClass`, so expect new errors.
- PHPUnit: only the 12 and 13 configs remain. Remove `--display-*` options from your scripts.
- PHPStan: the stricter checks above are new, so expect new errors.
- Phan: requires Phan 6. `UnusedSuppressionPlugin` is enabled.

---

## License

[MIT](LICENSE)
