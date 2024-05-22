### Comparison of Selenium, Puppeteer, Cypress with Playwright

#### 1. **Browser Support**

- **Playwright**: Supports Chromium, Firefox, and WebKit, making it flexible and modern.
- **Puppeteer**: Supports only Chromium-based browsers (Electron, new Edge).
- **Cypress**: Supports Chromium-based and Firefox, but limited support for other browsers.
- **Selenium**: Supports more browsers (Chrome, Firefox, Edge, IE, Safari, Opera), but is more complex to set up.

#### 2. **Programming Language Support**

- **Playwright**: Supports Java, Python, C#, JavaScript, TypeScript.
- **Puppeteer**: Only JavaScript and TypeScript.
- **Cypress**: Only JavaScript and TypeScript.
- **Selenium**: Supports multiple languages (Java, Python, C#, Ruby, JavaScript).

#### 3. **Cross-Platform Support**

- All tools support Windows, macOS, and Linux.

#### 4. **Speed and Stability**

- **Puppeteer**: Faster due to the DevTools protocol.
- **Playwright**: Faster and more stable due to the DevTools protocol for multiple browsers.
- **Cypress**: High performance for supported browsers.
- **Selenium**: Can be slow due to using WebDriver.

#### 5. **Features**

- **Playwright**: Modern, multi-browser, automatic waiting for elements, mobile device support.
- **Selenium**: Most mature and flexible, but more complex to set up and slower.
- **Puppeteer**: Simplicity and high performance for Chrome, but limited to browsers.
- **Cypress**: Simplicity and high performance for front-end developers, but limited to browsers.

### Code Examples with Element Waiting

**Playwright (TypeScript)**:

```typescript
import { test, expect } from "@playwright/test";

test("example test with waiting", async ({ page }) => {
  await page.goto("https://example.com");
  const heading = await page.locator("h1");
  await expect(heading).toBeVisible();
  await expect(heading).toHaveText("Example Domain");
});
```

**Selenium (JavaScript)**:

```javascript
const { Builder, By, until } = require("selenium-webdriver");

(async function example() {
  let driver = await new Builder().forBrowser("firefox").build();
  try {
    await driver.get("https://example.com");

    // Waiting for the element to be visible
    let heading = await driver.wait(
      until.elementLocated(By.tagName("h1")),
      10000
    );
    await driver.wait(until.elementIsVisible(heading), 10000);

    // Checking the text of the element
    let text = await heading.getText();
    console.log(text === "Example Domain");
  } finally {
    await driver.quit();
  }
})();
```

**Cypress (JavaScript)**:

```javascript
describe("example test with waiting", () => {
  it("should display the correct heading", () => {
    cy.visit("https://example.com");

    // Checking the text of the element
    cy.get("h1").should("be.visible").and("have.text", "Example Domain");
  });
});
```

**Puppeteer (JavaScript)**:

```javascript
const puppeteer = require("puppeteer");

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto("https://example.com");

  // Waiting for the element to be visible
  await page.waitForSelector("h1", { visible: true });

  // Checking the text of the element
  const heading = await page.$eval("h1", (el) => el.textContent);
  console.log(heading === "Example Domain");

  await browser.close();
})();
```

### Additional Comparison Articles

- [General Comparison](https://www.testim.io/blog/puppeteer-selenium-playwright-cypress-how-to-choose/)
- [Performance Comparison](https://www.checklyhq.com/blog/cypress-vs-selenium-vs-playwright-vs-puppeteer-speed-comparison/)

---

[Back: Playwright UI](../sections/4_ui_mode.md)

[Next: Advanced Features](../sections/6_advanced_features.md)

[Contents](../sections.md)
