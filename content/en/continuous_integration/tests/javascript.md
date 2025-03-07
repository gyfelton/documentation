---
title: JavaScript and TypeScript Tests
kind: documentation
aliases:
  - /continuous_integration/setup_tests/javascript
further_reading:
    - link: "/continuous_integration/tests/containers/"
      tag: "Documentation"
      text: "Forwarding Environment Variables for Tests in Containers"
    - link: "/continuous_integration/tests"
      tag: "Documentation"
      text: "Explore Test Results and Performance"
    - link: "/continuous_integration/intelligent_test_runner/javascript"
      tag: "Documentation"
      text: "Speed up your test jobs with Intelligent Test Runner"
    - link: "/continuous_integration/troubleshooting/"
      tag: "Documentation"
      text: "Troubleshooting CI"
---

{{< site-region region="gov" >}}
<div class="alert alert-warning">CI Visibility is not available in the selected site ({{< region-param key="dd_site_name" >}}) at this time.</div>
{{< /site-region >}}

## Compatibility

Supported test frameworks:
* Jest >= 24.8.0
  * Only `jsdom` (in package `jest-environment-jsdom`) and `node` (in package `jest-environment-node`) are supported as test environments. Custom environments like `@jest-runner/electron/environment` in `jest-electron-runner` are not supported.
  * Only [`jest-circus`][1] is supported as [`testRunner`][2].
  * Jest >= 28 is only supported from `dd-trace>=2.7.0`.
  * [`test.concurrent`](#jests-testconcurrent) is not supported.
* Mocha >= 5.2.0
  * Mocha >= 9.0.0 has [partial support](#known-limitations).
  * Mocha [parallel mode](#mocha-parallel-tests) is not supported.
* Cucumber-js >= 7.0.0
  * Cucumber-js [parallel mode](#cucumber-parallel-tests) is not supported.
* Cypress >= 6.7.0
  * From `dd-trace>=1.4.0`.
* Playwright >= 1.18.0
  * From `dd-trace>=3.13.0` and `dd-trace>=2.26.0` for 2.x release line.

The instrumentation works at runtime, so any transpilers such as TypeScript, Webpack, or Babel are supported out-of-the-box.

### Test suite level visibility compatibility
[Test suite level visibility][3] is fully supported from `dd-trace>=3.14.0` and `dd-trace>=2.27.0`. Jest, Mocha, Playwright, Cypress, and Cucumber are supported.

* Jest >= 24.8.0
  * From `dd-trace>=3.10.0`.
  * Only [`jest-circus`][1] is supported as [`testRunner`][2].
* Mocha >= 5.2.0
  * From `dd-trace>=3.10.0` and `dd-trace>=2.12.0` for 2.x release line.
* Playwright >= 1.18.0
  * From `dd-trace>=3.13.0` and `dd-trace>=2.26.0` for 2.x release line.
* Cypress >= 6.7.0
  * From `dd-trace>=3.14.0` and `dd-trace>=2.27.0` for 2.x release line.
* Cucumber >= 7.0.0
  * From `dd-trace>=3.14.0` and `dd-trace>=2.27.0` for 2.x release line.

## Configuring reporting method

To report test results to Datadog, you need to configure the Datadog JavaScript library:

{{< tabs >}}

{{% tab "On-Premises CI Provider (Datadog Agent)" %}}

{{% ci-agent %}}

{{% /tab %}}

{{% tab "Cloud CI provider (Agentless)" %}}

<div class="alert alert-info">Agentless mode is available in Datadog JavaScript library versions >= 2.5.0</div>

{{% ci-agentless %}}

{{% /tab %}}
{{< /tabs >}}

## Installing the JavaScript tracer

To install the [JavaScript Tracer][4], run:

```bash
yarn add --dev dd-trace
```

For more information, see the [JavaScript Tracer installation documentation][5].

## Instrument your tests

{{< tabs >}}
{{% tab "Jest/Mocha" %}}
Set the `NODE_OPTIONS` environment variable to `-r dd-trace/ci/init`. Run your tests as you normally would, specifying the environment where the tests are run in the `DD_ENV` environment variable. For example, set `DD_ENV` to `local` when running tests on a developer workstation, or `ci` when running them on a CI provider:

```bash
NODE_OPTIONS="-r dd-trace/ci/init" DD_ENV=ci DD_SERVICE=my-javascript-app yarn test
```

**Note**: If you set a value for `NODE_OPTIONS`, make sure it does not overwrite `-r dd-trace/ci/init`. This can be done using the `${NODE_OPTIONS:-}` clause:

{{< code-block lang="json" filename="package.json" >}}
{
  "scripts": {
    "test": "NODE_OPTIONS=\"--max-old-space-size=12288 ${NODE_OPTIONS:-}\" jest"
  }
}
{{< /code-block >}}

### Adding custom tags to tests

You can add custom tags to your tests by using the current active span:

```javascript
  it('sum function can sum', () => {
    const testSpan = require('dd-trace').scope().active()
    testSpan.setTag('team_owner', 'my_team')
    // test continues normally
    // ...
  })
```

To create filters or `group by` fields for these tags, you must first create facets. For more information about adding tags, see the [Adding Tags][1] section of the Node.js custom instrumentation documentation.


### Adding custom metrics to tests

Just like tags, you can add custom metrics to your tests by using the current active span:

```javascript
  it('sum function can sum', () => {
    const testSpan = require('dd-trace').scope().active()
    testSpan.setTag('memory_allocations', 16)
    // test continues normally
    // ...
  })
```
Read more about custom metrics in [Add Custom Metrics Guide][2].

[1]: /tracing/trace_collection/custom_instrumentation/nodejs?tab=locally#adding-tags
[2]: /continuous_integration/guides/add_custom_metrics/?tab=javascripttypescript
{{% /tab %}}

{{% tab "Playwright" %}}
Set the `NODE_OPTIONS` environment variable to `-r dd-trace/ci/init`. Run your tests as you normally would, specifying the environment where the tests are run in the `DD_ENV` environment variable. For example, set `DD_ENV` to `local` when running tests on a developer workstation, or `ci` when running them on a CI provider:

```bash
NODE_OPTIONS="-r dd-trace/ci/init" DD_ENV=ci DD_SERVICE=my-javascript-app yarn test
```

**Note**: If you set a value for `NODE_OPTIONS`, make sure it does not overwrite `-r dd-trace/ci/init`. This can be done using the `${NODE_OPTIONS:-}` clause:

{{< code-block lang="json" filename="package.json" >}}
{
  "scripts": {
    "test": "NODE_OPTIONS=\"--max-old-space-size=12288 ${NODE_OPTIONS:-}\" jest"
  }
}
{{< /code-block >}}

### Adding custom tags to tests

Custom tags are not supported for Playwright.

### Adding custom metrics to tests

Custom metrics are not supported for Playwright.
{{% /tab %}}

{{% tab "Cucumber" %}}
Set the `NODE_OPTIONS` environment variable to `-r dd-trace/ci/init`. Run your tests as you normally would, specifying the environment where the tests are run in the `DD_ENV` environment variable. For example, set `DD_ENV` to `local` when running tests on a developer workstation, or `ci` when running them on a CI provider:

```bash
NODE_OPTIONS="-r dd-trace/ci/init" DD_ENV=ci DD_SERVICE=my-javascript-app yarn test
```

**Note**: If you set a value for `NODE_OPTIONS`, make sure it does not overwrite `-r dd-trace/ci/init`. This can be done using the `${NODE_OPTIONS:-}` clause:

{{< code-block lang="json" filename="package.json" >}}
{
  "scripts": {
    "test": "NODE_OPTIONS=\"--max-old-space-size=12288 ${NODE_OPTIONS:-}\" jest"
  }
}
{{< /code-block >}}

### Adding custom tags to tests

You can add custom tags to your test by grabbing the current active span:

```javascript
  When('the function is called', function () {
    const stepSpan = require('dd-trace').scope().active()
    testSpan.setTag('team_owner', 'my_team')
    // test continues normally
    // ...
  })
```

To create filters or `group by` fields for these tags, you must first create facets. For more information about adding tags, see the [Adding Tags][1] section of the Node.js custom instrumentation documentation.


### Adding custom metrics to tests

You may also add custom metrics to your test by grabbing the current active span:

```javascript
  When('the function is called', function () {
    const stepSpan = require('dd-trace').scope().active()
    testSpan.setTag('memory_allocations', 16)
    // test continues normally
    // ...
  })
```
Read more about custom metrics in [Add Custom Metrics Guide][2]

[1]: /tracing/trace_collection/custom_instrumentation/nodejs?tab=locally#adding-tags
[2]: /continuous_integration/guides/add_custom_metrics/?tab=javascripttypescript
{{% /tab %}}

{{% tab "Cypress" %}}

### Cypress version 10 or later

Use the Cypress API documentation to [learn how to write plugins][1] for `cypress>=10`.

In your `cypress.config.js` file, set the following:

{{< code-block lang="javascript" filename="cypress.config.js" >}}
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    setupNodeEvents: require('dd-trace/ci/cypress/plugin'),
    supportFile: 'cypress/support/e2e.js'
  }
})
{{< /code-block >}}

Add the following line to the **top level** of your `supportFile`:

{{< code-block lang="javascript" filename="cypress/support/e2e.js" >}}
// Your code can be before this line
// require('./commands')
require('dd-trace/ci/cypress/support')
// Also supported:
// import 'dd-trace/ci/cypress/support'
// Your code can also be after this line
// Cypress.Commands.add('login', (email, pw) => {})
{{< /code-block >}}

If you're using other Cypress plugins, your `cypress.config.js` file should contain the following:

{{< code-block lang="javascript" filename="cypress.config.js" >}}
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      // your previous code is before this line
      require('dd-trace/ci/cypress/plugin')(on, config)
    }
  }
})
{{< /code-block >}}
<div class="alert alert-warning"> Datadog requires the <a href="#cypress-afterrun-event">after:run</a> Cypress event to work, and Cypress does not allow multiple <a href="">'after:run'</a> handlers. If you are using this event, dd-trace will not work properly.</div>

### Cypress before version 10

These are the instructions if you're using a version older than `cypress@10`.

1. Set [`pluginsFile`][2] to `"dd-trace/ci/cypress/plugin"`, for example, through [`cypress.json`][3]:
   {{< code-block lang="json" filename="cypress.json" >}}
   {
     "pluginsFile": "dd-trace/ci/cypress/plugin"
   }
   {{< /code-block >}}

   If you've already defined a `pluginsFile`, you can still initialize the instrumentation with:
   {{< code-block lang="javascript" filename="cypress/plugins/index.js" >}}
   module.exports = (on, config) => {
     // your previous code is before this line
     require('dd-trace/ci/cypress/plugin')(on, config)
   }
   {{< /code-block >}}
   <div class="alert alert-warning"> Datadog requires the <a href="#cypress-afterrun-event">'after:run'</a> Cypress event to work, and Cypress does not allow multiple <a href="">'after:run'</a> handlers. If you are using this event, dd-trace will not work properly.</div>

2. Add the following line to the **top level** of your [`supportFile`][4]:
   {{< code-block lang="javascript" filename="cypress/support/index.js" >}}
   // Your code can be before this line
   // require('./commands')
   require('dd-trace/ci/cypress/support')
   // Your code can also be after this line
   // Cypress.Commands.add('login', (email, pw) => {})
   {{< /code-block >}}


Run your tests as you normally do, specifying the environment where test are being run (for example, `local` when running tests on a developer workstation, or `ci` when running them on a CI provider) in the `DD_ENV` environment variable. For example:

{{< code-block lang="shell" >}}
DD_ENV=ci DD_SERVICE=my-ui-app npm test
{{< /code-block >}}


### Adding custom tags to tests

To add additional information to your tests, such as the team owner, use `cy.task('dd:addTags', { yourTags: 'here' })` in your test or hooks.

For example:

```javascript
beforeEach(() => {
  cy.task('dd:addTags', {
    'before.each': 'certain.information'
  })
})
it('renders a hello world', () => {
  cy.task('dd:addTags', {
    'team.owner': 'ui'
  })
  cy.get('.hello-world')
    .should('have.text', 'Hello World')
})
```

To create filters or `group by` fields for these tags, you must first create facets. For more information about adding tags, see the [Adding Tags][5] section of the Node.js custom instrumentation documentation.

### Adding custom metrics to tests

To add custom metrics to your tests, such as memory allocations, use `cy.task('dd:addTags', { yourNumericalTags: 1 })` in your test or hooks.

For example:

```javascript
it('renders a hello world', () => {
  cy.task('dd:addTags', {
    'memory_allocations': 16
  })
  cy.get('.hello-world')
    .should('have.text', 'Hello World')
})
```

Read more about custom metrics in [Add Custom Metrics Guide][6].

### Cypress - RUM integration

If the browser application being tested is instrumented using [Browser Monitoring][7], the Cypress test results and their generated RUM browser sessions and session replays are automatically linked. For more information, see the [Instrumenting your browser tests with RUM guide][8].


[1]: https://docs.cypress.io/api/plugins/writing-a-plugin#Plugins-API
[2]: https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests#Plugins-file
[3]: https://docs.cypress.io/guides/references/configuration#cypress-json
[4]: https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests#Support-file
[5]: /tracing/trace_collection/custom_instrumentation/nodejs?tab=locally#adding-tags
[6]: /continuous_integration/guides/add_custom_metrics/?tab=javascripttypescript
[7]: /real_user_monitoring/browser/#setup
[8]: /continuous_integration/guides/rum_integration/
{{% /tab %}}

{{< /tabs >}}

### Using Yarn 2 or later

If you're using `yarn>=2` and a `.pnp.cjs` file, and you get the following error message when using `NODE_OPTIONS`:

```text
 Error: Cannot find module 'dd-trace/ci/init'
```

You can fix it by setting `NODE_OPTIONS` to the following:

```bash
NODE_OPTIONS="-r $(pwd)/.pnp.cjs -r dd-trace/ci/init" yarn test
```

## Reporting code coverage

When tests are instrumented with [Istanbul][6], the Datadog Tracer (v3.20.0 or later) reports it under the `test.code_coverage.lines_pct` tag for your test sessions.

You can see the evolution of the test coverage in the **Coverage** tab of a test session.

Read more about code coverage in Datadog in [code coverage in Datadog guide][7].

## Configuration settings

The following is a list of the most important configuration settings that can be used with the tracer.

`service`
: Name of the service or library under test.<br/>
**Environment variable**: `DD_SERVICE`<br/>
**Default**: (test framework name)<br/>
**Example**: `my-ui`

`env`
: Name of the environment where tests are being run.<br/>
**Environment variable**: `DD_ENV`<br/>
**Default**: `none`<br/>
**Examples**: `local`, `ci`

`url`
: Datadog Agent URL for trace collection in the form `http://hostname:port`.<br/>
**Environment variable**: `DD_TRACE_AGENT_URL`<br/>
**Default**: `http://localhost:8126`

All other [Datadog Tracer configuration][8] options can also be used.

## Collecting Git metadata

{{% ci-git-metadata %}}

## Git metadata upload

From `dd-trace>=3.15.0` and `dd-trace>=2.28.0`, CI Visibility automatically uploads git metadata information (commit history). This metadata contains file names but no file contents. If you want to opt out of this behavior, you can do so by setting `DD_CIVISIBILITY_GIT_UPLOAD_ENABLED` to `false`. However, this is not recommended, as features like Intelligent Test Runner and others do not work without it.


## Manual testing API

<div class="alert alert-warning">
  <strong>Note</strong>: To use the manual testing API, you must pass <code>DD_CIVISIBILITY_MANUAL_API_ENABLED=1</code> as an environment variable.
</div>

<div class="alert alert-warning">
  <strong>Note</strong>: The manual testing API is in <strong>beta</strong>, so its API might change. It is available starting in <code>dd-trace</code> versions <code>4.4.0</code>, <code>3.25.0</code>, and <code>2.38.0</code>.
</div>

If you use Jest, Mocha, Cypress, Playwright, or Cucumber, **do not use the manual testing API**, as CI Visibility automatically instruments them and sends the test results to Datadog. The manual testing API is **incompatible** with already supported testing frameworks.

Use the manual testing API only if you use an unsupported testing framework or have a different testing mechanism.

The manual testing API leverages the `node:diagnostics_channel` module from Node.js and is based on channels you can publish to:

```javascript
const { channel } = require('node:diagnostics_channel')

const { describe, test, beforeEach, afterEach, assert } = require('my-custom-test-framework')

const testStartCh = channel('dd-trace:ci:manual:test:start')
const testFinishCh = channel('dd-trace:ci:manual:test:finish')
const testSuite = __filename

describe('can run tests', () => {
  beforeEach((testName) => {
    testStartCh.publish({ testName, testSuite })
  })
  afterEach((status, error) => {
    testFinishCh.publish({ status, error })
  })
  test('first test will pass', () => {
    assert.equal(1, 1)
  })
})
```

### Test start channel

Grab this channel by its ID `dd-trace:ci:manual:test:start` to publish that a test is starting. A good place to do this is a `beforeEach` hook or similar.

```typescript
const { channel } = require('node:diagnostics_channel')
const testStartCh = channel('dd-trace:ci:manual:test:start')

// ... code for your testing framework goes here
  beforeEach(() => {
    const testDefinition = {
      testName: 'a-string-that-identifies-this-test',
      testSuite: 'what-suite-this-test-is-from.js'
    }
    testStartCh.publish(testDefinition)
  })
// code for your testing framework continues here ...
```

The payload to be published has attributes `testName` and `testSuite`, both strings, that identify the test that is about to start.

### Test finish channel

Grab this channel by its ID `dd-trace:ci:manual:test:finish` to publish that a test is ending. A good place to do this is an `afterEach` hook or similar.

```typescript
const { channel } = require('node:diagnostics_channel')
const testFinishCh = channel('dd-trace:ci:manual:test:finish')

// ... code for your testing framework goes here
  afterEach(() => {
    const testStatusPayload = {
      status: 'fail',
      error: new Error('assertion error')
    }
    testStartCh.publish(testStatusPayload)
  })
// code for your testing framework continues here ...
```

The payload to be published has attributes `status` and `error`:

* `status` is a string that takes one of three values:
  * `'pass'` when a test passes.
  * `'fail'` when a test fails.
  * `'skip'` when a test has been skipped.

* `error` is an `Error` object containing the reason why a test failed.

### Add tags channel

Grab this channel by its ID `dd-trace:ci:manual:test:addTags` to publish that a test needs custom tags. This can be done within the test function:

```typescript
const { channel } = require('node:diagnostics_channel')
const testAddTagsCh = channel('dd-trace:ci:manual:test:addTags')

// ... code for your testing framework goes here
  test('can sum', () => {
    testAddTagsCh.publish({ 'test.owner': 'my-team', 'number.assertions': 3 })
    const result = sum(2, 1)
    assert.equal(result, 3)
  })
// code for your testing framework continues here ...
```

The payload to be published is a dictionary `<string, string|number>` of tags or metrics that are added to the test.


### Run the tests

When the test start and end channels are in your code, run your testing framework like you normally do, including the following environment variables:

```shell
NODE_OPTIONS="-r dd-trace/ci/init" DD_CIVISIBILITY_MANUAL_API_ENABLED=1 DD_ENV=ci DD_SERVICE=my-custom-framework-tests yarn run-my-test-framework
```



## Known limitations

### ES modules
[Mocha >=9.0.0][9] uses an ESM-first approach to load test files. That means that if [ES modules][10] are used (for example, by defining test files with the `.mjs` extension), _the instrumentation is limited_. Tests are detected, but there isn't visibility into your test. For more information about ES modules, see the [Node.js documentation][10].

### Browser tests
Browser tests executed with `mocha`, `jest`, `cucumber`, `cypress`, and `playwright` are instrumented by `dd-trace-js`, but visibility into the browser session itself is not provided by default (for example, network calls, user actions, page loads, and more.).

If you want visibility into the browser process, consider using [RUM & Session Replay][11]. When using Cypress, test results and their generated RUM browser sessions and session replays are automatically linked. For more information, see the [Instrumenting your browser tests with RUM guide][12].

### Cypress interactive mode

Cypress interactive mode (which you can enter by running `cypress open`) is not supported by CI Visibility because some cypress events, such as [`before:run`][13], are not fired. If you want to try it anyway, pass `experimentalInteractiveRunEvents: true` to the [cypress configuration file][14].

### Cypress `after:run` event

Datadog requires usage of the Cypress [`after:run` event][15]. Cypress only allows a single listener for this event, so if your custom Cypress plugin requires `after:run`, it is incompatible with `dd-trace`.

### Mocha parallel tests
Mocha's [parallel mode][16] is not supported. Tests run in parallel mode are not instrumented by CI Visibility.

### Cucumber parallel tests
Cucumber's [parallel mode][17] is not supported. Tests run in parallel mode are not instrumented by CI Visibility.

### Jest's `test.concurrent`
Jest's [test.concurrent][18] is not supported.

## Best practices

Follow these practices to take full advantage of the testing framework and CI Visibility.

### Parameterized tests

Whenever possible, leverage the tools that testing frameworks provide for parameterized tests. For example, for `jest`:

Avoid this:
{{< code-block lang="javascript" >}}
[[1,2,3], [3,4,7]].forEach((a,b,expected) => {
  test('sums correctly', () => {
    expect(a+b).toEqual(expected)
  })
})
{{< /code-block >}}

And use [`test.each`][19] instead:

{{< code-block lang="javascript" >}}
test.each([[1,2,3], [3,4,7]])('sums correctly %i and %i', (a,b,expected) => {
  expect(a+b).toEqual(expected)
})
{{< /code-block >}}

For `mocha`, use [`mocha-each`][20]:

{{< code-block lang="javascript" >}}
const forEach = require('mocha-each');
forEach([
  [1,2,3],
  [3,4,7]
])
.it('adds %i and %i then returns %i', (a,b,expected) => {
  expect(a+b).to.equal(expected)
});
{{< /code-block >}}

When you use this approach, both the testing framework and CI Visibility can tell your tests apart.

{{% ci-information-collected %}}

In addition to that, if [Intelligent Test Runner][21] is enabled, the following data is collected from your project:

* Code coverage information, including file names and line numbers covered by each test.

## Further reading

{{< partial name="whats-next/whats-next.html" >}}


[1]: https://github.com/facebook/jest/tree/main/packages/jest-circus
[2]: https://jestjs.io/docs/configuration#testrunner-string
[3]: /continuous_integration/tests/#test-suite-level-visibility
[4]: /tracing/trace_collection/dd_libraries/nodejs
[5]: https://github.com/DataDog/dd-trace-js#version-release-lines-and-maintenance
[6]: https://istanbul.js.org/
[7]: /continuous_integration/guides/code_coverage/?tab=javascripttypescript
[8]: /tracing/trace_collection/library_config/nodejs/?tab=containers#configuration
[9]: https://github.com/mochajs/mocha/releases/tag/v9.0.0
[10]: https://nodejs.org/api/packages.html#packages_determining_module_system
[11]: /real_user_monitoring/browser/
[12]: /continuous_integration/guides/rum_integration/
[13]: https://docs.cypress.io/api/plugins/before-run-api
[14]: https://docs.cypress.io/guides/references/configuration#Configuration-File
[15]: https://docs.cypress.io/api/plugins/after-run-api
[16]: https://mochajs.org/#parallel-tests
[17]: https://github.com/cucumber/cucumber-js/blob/63f30338e6b8dbe0b03ddd2776079a8ef44d47e2/docs/parallel.md
[18]: https://jestjs.io/docs/api#testconcurrentname-fn-timeout
[19]: https://jestjs.io/docs/api#testeachtablename-fn-timeout
[20]: https://www.npmjs.com/package/mocha-each
[21]: /continuous_integration/intelligent_test_runner/
