# @testcafe-community/axe
The TestCafe module that allows you to use the [aXe](https://github.com/dequelabs/axe-core) accessibility engine in TestCafe tests.

## Installation

```bash
yarn add -D axe-core @testcafe-community/axe
```

## How to use

You can write a TestCafe test with automated accessibility checks like this.

Add the following clientScript in your testcafe config:

```js
"clientScripts":[{"module":"axe-core/axe.min.js"}]
```

```js
import { checkForViolations } from '@testcafe-community/axe';

fixture `TestCafe tests with Axe`
    .page `http://example.com`;

test('Automated accessibility testing', async t => {
    // do stuff on your page
    await checkForViolations();
});
```

If any accessibility issues are found, you will see a detailed report in the error log.

![Accessibility errors](https://github.com/testcafe-community/axe/blob/master/errors.png)

## aXe options

The `@testcafe-community/axe` module allows you to define the `context` and `options` [axe.run parameters](https://github.com/dequelabs/axe-core/blob/develop/doc/API.md#api-name-axerun) in a TestCafe test.

```js
test('Automated accessibility testing', async () => {
    const context = { exclude: [['select']] };
    const options = { rules: { 'html-has-lang': { enabled: false } } };

    await checkForViolations({context, options});
});
```

## Additional features

By default `checkForViolations` does not allow any violations, but if you want more fine grained control 
there are a couple options.  First, you can pass in `numAllowed` which will only fail the test if the 
number of violations exceeds that number.  Secondly, `checkViolations` returns the list of violations from
axe-core so you can inspect it if needed.

```js
test('Automated accessibility testing', async () => {
    const {violations} = await checkForViolations({numAllowed:2});

    // do stuff with violations.
});
```

## Legacy API

This project was forked from [axe-testcafe](https://github.com/helen-dikareva/axe-testcafe) which has been dormant for quite some time.  If you prefer to use that API you can still use that:

```js
import { axeCheck, createReport } from 'axe-testcafe';

fixture `TestCafe tests with Axe`
    .page `http://example.com`;

test('Automated accessibility testing', async t => {
    const { error, violations } = await axeCheck(t);
    await t.expect(violations.length === 0).ok(createReport(violations));
});
```
