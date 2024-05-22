# Advanced Features

## UI Mode in Playwright

UI Mode in Playwright is a tool that provides a graphical interface for running and debugging tests. It allows you to visually observe the execution of tests, analyze results, and interact with the browser in real-time. This mode is particularly useful for developers as it simplifies the debugging and testing process.

### Key Features of UI Mode

1. **Test Visualization**: You can watch tests execute in real-time, seeing how the browser performs each action.
2. **Interactive Debugging**: The ability to pause tests, interact with the browser, and check the state of elements on the page.
3. **Result Analysis**: In UI Mode, you can view test results, including errors, screenshots, and traces.

### Running UI Mode

To launch UI Mode, use the command:

```bash
npx playwright test --ui
```

This command will open the UI Mode interface in the browser, where you can select tests to run and analyze their results.

### Main Components of UI Mode

1. **Test List**: Displays all tests available in your project. You can run individual tests or test suites.
2. **Console and Terminal**: The built-in console displays logs and test results.
3. **Screenshots and Videos**: Allows viewing screenshots and videos recorded during test execution.
4. **Traces**: Trace visualization helps analyze test execution steps, network requests, and element interactions.

### Example Usage of UI Mode

1. Open the terminal and navigate to your Playwright project directory.
2. Launch UI Mode with the command `npx playwright test --ui`.
3. In the opened interface, select the tests you want to run.
4. Observe the test execution in real-time. You can pause tests and interact with the browser for debugging.

### Benefits of Using UI Mode

- **Convenient Debugging**: The visual interface allows for faster and more efficient problem identification in tests.
- **Detailed Reports**: The ability to view screenshots, videos, and traces makes it easier to analyze test results.
- **Interactivity**: The ability to pause tests and interact with the browser simplifies the debugging and testing process.

[Playwright Documentation on UI Mode](https://playwright.dev/docs/test-ui-mode)

## Debugging in Playwright

Playwright provides several powerful tools for debugging tests, including the interactive `--debug` mode and integration with Visual Studio Code (VS Code).

### Debugging with the `--debug` Command

Playwright's debug mode can be launched with the `--debug` command. This allows you to pause test execution, open DevTools, and manually interact with the browser.

#### Running Tests in Debug Mode

Launch tests with the `--debug` flag:

```bash
npx playwright test --debug
```

After running this command, tests will pause before execution, and a browser window with DevTools will open. You can step through the tests, set breakpoints, and analyze the current state of the browser.

#### Using `page.pause()`

You can also add the `page.pause()` method in your test to pause execution at a specific point:

```typescript
import { test, expect } from "@playwright/test";

test("Pause example", async ({ page }) => {
  await page.goto("https://example.com");
  await page.pause(); // Pause test execution and open DevTools
  const title = await page.title();
  expect(title).toBe("Example Domain");
});
```

### Debugging with Visual Studio Code (VS Code)

Playwright offers integration with VS Code, enabling convenient test debugging directly within the editor.

#### Installing the Playwright Extension for VS Code

1. Open VS Code.
2. Go to the Extensions tab or press `Ctrl+Shift+X`.
3. Search for and install the **Playwright Test for VSCode** extension.

#### Additional Features

- **Built-in Environment Variables**: You can configure environment variables in the launch configuration for different environments.
- **Console and Terminal**: Debug information is displayed in the integrated console or terminal in VS Code, making it easier to analyze logs and errors.

## Sharding in Playwright

Sharding is a method of dividing a test suite into several parts (shards) that can be run in parallel on different machines or processes. This significantly reduces the overall test execution time and improves testing efficiency.

### Example of Using Sharding

To split tests into multiple shards, use the `--shard=x/y` parameter in the command line. For example, to divide tests into four shards, each executing a quarter of the tests:

```bash
npx playwright test --shard=1/4
npx playwright test --shard=2/4
npx playwright test --shard=3/4
npx playwright test --shard=4/4
```

Running these shards in parallel on different machines allows you to complete the test suite four times faster.

### Sharding Configuration

You can set sharding parameters in the `playwright.config.ts` configuration file:

```typescript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  testDir: "./tests", // Directory containing tests
  retries: 2, // Number of retries on failure
  workers: 4, // Number of parallel workers
  shard: {
    total: 2, // Total number of shards
    current: 1, // Current shard number (starting from 1)
  },
  use: {
    headless: true, // Run tests in headless mode
    viewport: { width: 1280, height: 720 }, // Browser window size
    screenshot: "only-on-failure", // Capture screenshots only on test failure
    trace: "on-first-retry", // Record trace on first retry
  },
});
```

### Merging Reports from Multiple Shards

Each test shard generates its own report. To obtain a combined report showing results from all shards, you can merge them. To do this, add the `blob` reporter in the configuration:

To merge reports from multiple shards, gather the report files into a single directory, such as `all-blob-reports`, and then run the `npx playwright merge-reports` command:

```bash
npx playwright merge-reports --reporter=html ./all-blob-reports
```

This will create a standard HTML report in the `playwright-report` directory.

[Playwright Documentation on Sharding](https://playwright.dev/docs/test-sharding)

---

[Back: Comparison with Other Tools](../sections/5_comparing.md)

[Next: Potentially Useful Tips](../sections/7_maybe_usefull.md)

[Contents](../sections.md)
