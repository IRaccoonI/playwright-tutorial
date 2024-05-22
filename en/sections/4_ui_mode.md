# Playwright UI

Playwright UI Mode is a visual interface that provides developers with a convenient way to run and debug tests written using Playwright. This mode allows you to see test results in real-time, interact with the browser, explore elements on the page, and manage test scenarios through an intuitive graphical interface.

[Documentation](https://playwright.dev/docs/test-ui-mode)

## Key Features of Playwright UI:

1. **Running and Debugging Tests**:

   - You can run tests and see their results in real-time.
   - Step-by-step debugging with breakpoints helps you analyze test behavior in detail and find errors.

2. **Element Inspection**:

   - A visual tool for exploring the DOM tree.
   - The ability to select elements on the page and view their properties, such as attributes, CSS styles, and events.

3. **Recording and Generating Tests**:

   - The ability to record user actions and automatically generate test code based on these actions.
   - Convenient for creating tests without manually writing code.

4. **Working with Multiple Browsers**:
   - Support for working with multiple browsers simultaneously, such as Chromium, Firefox, and WebKit.
   - A user-friendly interface for switching between different browsers and their configurations.

## Installation and Launching Playwright UI

To use Playwright UI, you need to install Playwright if it is not already installed:

```sh
npm install @playwright/test
```

After installing Playwright, you can launch the UI with the following command:

```sh
npx playwright test --ui
```

This will launch Playwright Test in UI mode and open the interface in the browser. In this interface, you will be able to see a list of tests, their results, and manage test scenarios.

## Examples of Using Playwright UI

1. **Running Tests**:

   - Select the tests you want to run and click the run button.
   - The tests will execute, and you will be able to see their progress and results in real-time.

2. **Debugging Tests**:

   - During test execution, you can set breakpoints to pause execution and explore the application state.
   - Use step-by-step debugging tools to understand what happens at each step of the test.

3. **Inspecting and Interacting with Elements**:

   - Select an element on the page to inspect it.
   - View and modify element properties to understand how they affect the tests.

---

[Back: Core Concepts](../sections/3_core_concepts.md)

[Next: Comparison with Other Tools](../sections/5_comparing.md)

[Contents](../sections.md)
