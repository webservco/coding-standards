# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

`webservco/coding-standards` is a Composer package (`type: phpcodesniffer-standard`) that contains **only shared configuration files** for PHP static analysis and testing tools. It has no PHP source code, no tests and no build step. Consuming projects install it as a dev dependency and point each tool at `vendor/webservco/coding-standards/<tool>/<file>`. See `README.md` for the per-tool usage commands used in those projects.

Because of this, every path inside these configs is resolved **from the consuming project's root**, not from this repo. Examples: phpcs rulesets reference `vendor/slevomat/coding-standard/...`, PHPStan includes use `%currentWorkingDirectory%/vendor/...`, and Psalm/Phan list project directories such as `bin config public resources src tests`. Keep that convention when editing. The tools can't be run against this repo directly in any meaningful way.

## Layout and conventions

- `phpcs/`:
  - `ruleset-psr.xml`: PSR12 only.
  - `ruleset-psr-slevomat.xml`: the same, plus the full Slevomat ruleset with a curated list of `<exclude>`s and property overrides (e.g. `FunctionLength`, `ReferenceUsedNamesOnly`, `AttributesOrder`).
  - `ruleset-namespaces.xml`: a standalone set of Slevomat namespace/use rules.
- `phpstan/`: `phpstan.neon` is the base config (level max, strict, deprecation and phpunit rules). `phpstan-symfony.neon` includes the base file and adds the Symfony extension and paths; `phpstan-symfony-doctrine.neon` includes the Symfony file and adds Doctrine. Neon `includes` of sibling files are relative to the config file, but `paths` and other locations must use `%currentWorkingDirectory%`.
- `psalm/`: base, symfony and symfony-doctrine variants. Each variant repeats the base settings.
- `phpmd/phpmd-rule-set.xml`, `phan/config.php`: one config each.
- `phpmd/phpmd-rule-set.xml` targets PHPMD 3 (`composer.json` declares a conflict with `phpmd/phpmd <3`). To configure a rule, exclude it from its rule set and reference it again with properties: PHPMD 3 accepts a property override without the exclude, but with XML rule sets the default rule instance keeps running next to the configured one.
- `phpunit/`: one config per PHPUnit major version (12, 13).

### Editing the phpcs rulesets

Rule changes go in `ruleset-psr.xml` or `ruleset-psr-slevomat.xml`. The rulesets are not versioned per PHP version; do not add versioned copies.

Every `<exclude>` has a comment that gives the reason (personal preference, PSR12 conflict, etc.) and quotes the sniff's error message. Follow the same format for new excludes.

## Verifying changes

There is no test suite. To check an edit, install dependencies (`composer install`) and run the tool against a sample PHP project that uses this package, or point the tool at the edited file from a consuming project. As a quick check, XML files can be validated with `xmllint --noout <file>`.
