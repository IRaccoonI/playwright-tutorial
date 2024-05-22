# Installation and Setup

## Installing Playwright

### Requirements

To work with Playwright, you need to have Node.js installed. Ensure you have the latest version of Node.js, which can be downloaded from the [official Node.js website](https://nodejs.org/).

### Installing Playwright

To install Playwright in your project, follow these steps:

#### Install Playwright using npm:

```bash
npm install playwright
npx playwright install
```

Playwright also provides a quick setup through the `init` command, which automatically configures your project, installs all necessary browsers, and creates the project structure.

## Project Setup

### Configuration Files

Playwright allows you to configure the project using the `playwright.config.js` file. This file is used to set up test launch parameters, browsers, and parallel execution. Here's an example of a basic configuration:

```javascript
const { devices } = require("@playwright/test");

module.exports = {
  testDir: "./tests", // Directory with tests
  timeout: 30000, // Timeout for each test
  retries: 2, // Number of retries on failure
  outputDir: "./test-results", // Directory for test results
  workers: 4, // Number of parallel workers
  projects: [
    {
      name: "Chromium",
      use: {
        browserName: "chromium",
        headless: true, // Run tests in headless mode
        viewport: { width: 1280, height: 720 }, // Browser window size
      },
    },
    {
      name: "Firefox",
      use: {
        browserName: "firefox",
        headless: false, // Run tests in windowed mode
      },
    },
    {
      name: "WebKit",
      use: {
        browserName: "webkit",
        ignoreHTTPSErrors: true, // Ignore HTTPS errors
      },
    },
    {
      name: "Mobile Safari",
      use: devices["iPhone 12"], // Use settings for a mobile device
    },
  ],
  use: {
    baseURL: "https://example.com", // Base URL for tests
    headless: true, // Run tests in headless mode
    viewport: { width: 1280, height: 720 }, // Browser window size
    ignoreHTTPSErrors: true, // Ignore HTTPS errors
    video: "retain-on-failure", // Record video only on test failures
    screenshot: "only-on-failure", // Take screenshots only on test failures
    trace: "on-first-retry", // Record trace on the first retry
  },
  reporter: [
    ["list"], // Standard console output
    ["json", { outputFile: "test-results/results.json" }], // JSON reporter
    ["html", { open: "never" }], // HTML reporter
    ["junit", { outputFile: "test-results/results.xml" }], // JUnit reporter
    ["line"], // Line reporter
    ["allure-playwright"], // Integration with Allure reporting
  ], // Reporter configuration
  webServer: {
    command: "npm run start", // Command to start the web server
    url: "http://0.0.0.0:3000", // Web server address for health check
    timeout: 120 * 1000, // Timeout for server startup
    reuseExistingServer: !process.env.CI, // Use existing server if available, otherwise start a new one
  },
  expect: {
    timeout: 5000, // Timeout for expect waits in milliseconds
    toHaveScreenshot: {
      threshold: 0.2, // Threshold value for screenshot differences
      animations: "disabled", // Disable animations for stable screenshots
      scale: "css", // Scale the image according to CSS
    },
    toMatchSnapshot: {
      threshold: 0.2, // Threshold value for snapshot differences
      maxDiffPixels: 100, // Maximum allowable number of differing pixels
    },
  },
};
```

- [Playwright configuration docs](https://playwright.dev/docs/test-configuration)
- [allure-playwright](https://allurereport.org/docs/playwright/)
- [junit photos](../assets/junit.md)

### Mobile Device Testing in Playwright

Playwright supports mobile device emulation, allowing you to test web applications on various devices with different screen sizes and characteristics. This makes Playwright a versatile tool for end-to-end testing, covering both desktop and mobile platforms.

#### Key Features of Mobile Device Testing in Playwright

1. **Device Emulation**: Playwright allows emulation of popular mobile devices like iPhone and Android devices, considering their screen sizes, user agents, and other characteristics.
2. **Real Browser Testing**: Emulation is performed in real browsers (Chromium, Firefox, WebKit), ensuring high test accuracy.
3. **Geolocation and Sensors**: Support for emulating geolocation and other sensors, such as accelerometer.

#### Example of Mobile Device Emulation in Playwright

```typescript
import { test, devices } from "@playwright/test";

const iPhone11 = devices["iPhone 11"];

test.use({ ...iPhone11 });

test("Test on iPhone 11", async ({ page }) => {
  await page.goto("https://example.com");
  // Your tests here
});
```

#### Differences from Testing in Desktop Browsers

1. **Screen Size and Resolution**: Mobile devices have smaller screens and different resolutions, affecting content display.
2. **User Agents**: Mobile browsers use different user agents, which can influence web page behavior.
3. **Sensors and Orientation**: The ability to test interactions with sensors like accelerometer and geolocation, which have no desktop equivalents.

#### Availability of Similar Features in Other Tools

1. **Selenium**: Also supports mobile device emulation through RemoteWebDriver and using Appium for more precise mobile application emulation.
2. **Cypress**: Offers limited support for mobile device emulation, mainly through screen size and user agent changes.
3. **Puppeteer**: Mainly focused on working with Chromium, while Playwright supports not only Chromium but also Firefox and WebKit.

### Creating Project Structure

Create the following directory structure to store tests and configurations:

```
my-playwright-project/
├── tests/
│   └── example.spec.js
├── node_modules/
├── package.json
├── playwright.config.js
└── README.md
```

### Example Test

Create a file `example.spec.js` in the `tests` directory and add the following example test:

```javascript
const { test, expect } = require("@playwright/test");

test("basic test", async ({ page }) => {
  await page.goto("https://example.com");
  const title = await page.title();
  expect(title).toBe("Example Domain");
});
```

### Running Tests

To run the tests, use the command:

```bash
npx playwright test
```

This command will find all tests in the `tests` directory and execute them.

---

[Back: Introduction](../sections/1_introduction.md)

[Next: Core Concepts](../sections/3_core_concepts.md)

[Contents](../sections.md)
