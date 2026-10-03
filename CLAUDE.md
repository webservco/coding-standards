# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

`webservco/coding-standards` is a Composer package (`type: phpcodesniffer-standard`) that contains **only shared configuration files** for PHP static analysis and testing tools. It has no PHP source code, no tests and no build step. Consuming projects install it as a dev dependency and point each tool at `vendor/webservco/coding-standards/<tool>/<file>`. See `README.md` for the per-tool usage commands used in those projects.

Because of this, every path inside these configs is resolved **from the consuming project's root**, not from this repo. Examples: phpcs rulesets reference `vendor/slevomat/coding-standard/...`, PHPStan includes use `%currentWorkingDirectory%/vendor/...`, and Psalm/Phan list project directories such as `bin config public resources src tests`. Keep that convention when editing. The tools can't be run against this repo directly in any meaningful way.

## Layout and conventions

- `phpcs/`:
  - `ruleset-psr.xml`: PSR12 only.
  - `ruleset-psr-slevomat.xml`: the same, plus the full Slevomat ruleset with a curated list of `<exclude>`s and property overrides (e.g. `FunctionLength`, `ReferenceUsedNamesOnly`, `AttributesOrder`).
  - `ruleset-psr-phpXY.xml` / `ruleset-psr-phpXY-slevomat.xml` (8.3 and 8.4): deprecated wrappers that only include the matching unversioned ruleset via `<rule ref="./ruleset-psr[-slevomat].xml"/>`. They are kept because many projects reference them by name. Do not add rules to them and do not add new versioned files.
  - `ruleset-namespaces.xml`: a standalone set of Slevomat namespace/use rules.
- `phpstan/`: `phpstan.neon` is the base config (level max, strict, deprecation and phpunit rules). The `-symfony` and `-symfony-doctrine` variants repeat the base settings and add their own extensions/paths. They do not include the base file.
- `psalm/`: same pattern, with base, symfony and symfony-doctrine variants.
- `phpmd/phpmd-rule-set.xml`, `phan/config.php`: one config each.
- `phpunit/`: one config per PHPUnit major version (9, 10, 11, 12, 13).

### Editing the phpcs rulesets

Rule changes go in `ruleset-psr.xml` or `ruleset-psr-slevomat.xml`; the deprecated versioned wrappers pick them up automatically.

Every `<exclude>` has a comment that gives the reason (personal preference, PSR12 conflict, etc.) and quotes the sniff's error message. Follow the same format for new excludes.

## Verifying changes

There is no test suite. To check an edit, install dependencies (`composer install`) and run the tool against a sample PHP project that uses this package, or point the tool at the edited file from a consuming project. As a quick check, XML files can be validated with `xmllint --noout <file>`.
