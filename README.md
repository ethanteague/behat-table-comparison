# Behat Table Comparison

[![Build Status](https://travis-ci.org/TravisCarden/behat-table-comparison.svg?branch=develop)](https://travis-ci.org/TravisCarden/behat-table-comparison)
[![Code Climate](https://codeclimate.com/github/TravisCarden/behat-table-comparison/badges/gpa.svg)](https://codeclimate.com/github/TravisCarden/behat-table-comparison)
[![Test Coverage](https://codeclimate.com/github/TravisCarden/behat-table-comparison/badges/coverage.svg)](https://codeclimate.com/github/TravisCarden/behat-table-comparison/coverage)

The Behat Table Comparison library provides an equality assertion for comparing Behat `TableNode` tables.

## Installation & Usage

Install the library via [Composer](https://getcomposer.org/):

```bash
composer require --dev traviscarden/behat-table-comparison
```

Then use the [`TableEqualityAssertion`](src/BehatTableComparison/TableEqualityAssertion.php) class in your [`FeatureContext` class](http://docs.behat.org/en/v2.5/guides/4.context.html):

```php
<?php

use Behat\Behat\Context\Context;
use Behat\Behat\Tester\Exception\PendingException;
use Behat\Gherkin\Node\TableNode;
use TravisCarden\BehatTableComparison\TableEqualityAssertion;

class FeatureContext implements Context
{

    /**
     * @Given I am :author
     */
    public function doNothing($author)
    {
    }

    /**
     * @Then I should include the following :items in :group
     */
    public function iShouldIncludeTheFollowingIn($items, $group, TableNode $expected)
    {
        switch ($group) {
            case 'The Lord of the Rings series':
                $actual = new TableNode([
                    ['The Hobbit'],
                    ['The Fellowship of the Ring'],
                    ['The Two Towers'],
                ]);
                (new TableEqualityAssertion($expected, $actual))
                    ->setMissingRowsLabel('Missing books')
                    ->setUnexpectedRowsLabel('Unexpected books')
                    ->assert();
                break;

            default:
                throw new PendingException();
        }
    }
}

```

Output is like the following:

![Example Output](misc/example-output.gif)

## Examples

See [`features/bootstrap/FeatureContext.php`](features/bootstrap/FeatureContext.php) and [`features/examples.feature`](features/examples.feature) for more examples.

## Limitations & Known Issues

Some inequality detection currently works but does not yet display a helpful error message, because it has not been decided what it should show. Please help me [specify error messages for complex differences](https://github.com/TravisCarden/behat-table-comparison/issues/1).

## Contribution

All contributions are welcome according to [normal GitHub practice](https://guides.github.com/activities/contributing-to-open-source/#contributing).
