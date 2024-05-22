# Introduction

## What is Playwright?

Playwright is a browser automation framework by Microsoft designed for writing scripts that can automate interactions with web applications. It allows developers and testers to write tests that can simulate user actions such as navigating web pages, filling out forms, clicking on elements, and much more.

### Key Features of Playwright:

- **Support for Multiple Browsers**: Playwright supports automation for Chromium, Firefox, and WebKit. This allows testing applications in various browser engines, ensuring better cross-browser compatibility.
- **High Performance**: Playwright is known for its high speed in executing tests due to its optimized architecture.
- **Modern Capabilities**: Working with multiple tabs, intercepting network requests, capturing videos and screenshots, and automatic waiting for elements make it a powerful tool for end-to-end (E2E) testing.
- **Automatic Waiting**: Playwright automatically waits for elements to be available, reducing the need for explicit waits in test code.

## History and Developers

Playwright was created by Microsoft in response to the limitations developers and testers faced with other automation frameworks like Selenium and Cypress. Since its release, Playwright has quickly gained popularity due to its performance and powerful features.

## Why Use Playwright?

Using Playwright for E2E testing provides numerous advantages, including:

- **High Performance**: Fast test execution due to efficient handling of browser sessions and parallel test execution.
- **Intuitive API**: Easy to write and maintain tests thanks to a clear and well-documented API.

  ```typescript
  import { test, expect } from "@playwright/test";

  test("basic test", async ({ page }) => {
    await page.goto("https://example.com");
    const title = await page.title();
    expect(title).toBe("Example Domain");
  });
  ```

- **Support for Modern Web Standards**: Work with new browser features and web standards.

  ```typescript
  import { webkit } from "@playwright/test";
  (async () => {
    const browser = await webkit.launch();
    const page = await browser.newPage();
    await page.goto("https://example.com");
    await page.evaluate(() => {
      localStorage.setItem("myItem", "value"); // Working with Local Storage
    });
    const value = await page.evaluate(() => {
      return localStorage.getItem("myItem");
    });
    console.log(value); // Retrieve value from Local Storage
    await browser.close();
  })();
  ```

Playwright is becoming an increasingly popular choice for test automation due to its flexibility and powerful capabilities, enabling testers and developers to create reliable and stable tests for their web applications.

---

[Next: Installation and Setup](../sections/2_installation_and_setup.md)

[Contents](../sections.md)
