# Core Concepts

## Browsers and Contexts

### Supported Browsers

Playwright supports automation for the following browsers:

- Chromium
- Firefox
- WebKit

### Creating and Managing Contexts

Contexts in Playwright allow you to create isolated browser sessions, which are useful for running tests in parallel and working with different states. Here's an example of creating a context:

```typescript
import { test, expect } from "@playwright/test";

test("Browser context test", async ({ browser }) => {
  const context = await browser.newContext({
    extraHTTPHeaders: {
      Authorization: "Bearer my-token",
    },
    geolocation: { latitude: 37.7749, longitude: -122.4194 },
    locale: "en-US",
  });

  const page = await context.newPage();
  await page.goto("https://maps.example.com");

  // Check geolocation (more logic might be needed for a specific site)
  const geo = await page.evaluate(() => {
    return new Promise((resolve) => {
      navigator.geolocation.getCurrentPosition((position) => {
        resolve({
          latitude: position.coords.latitude,
          longitude: position.coords.longitude,
        });
      });
    });
  });
  expect(geo.latitude).toBe(37.7749);
  expect(geo.longitude).toBe(-122.4194);

  // Check Headers
  const response = await page.waitForResponse("https://example.com");
  const headers = await response.request().headers();
  expect(headers["authorization"]).toBe("Bearer my-token");

  await context.close();
});
```

## Pages and Frames

### Working with Pages

A page (`page`) is the main entity for interacting with web page content. You can open new pages, navigate to URLs, and interact with elements on the page.

```typescript
import { test, expect } from "@playwright/test";

test("Page navigation test", async ({ page }) => {
  await page.goto("https://example.com");
  const title = await page.title();
  expect(title).toBe("Example Domain");
});
```

### Navigating and Working with Frames

Frames allow you to work with embedded elements (iframe) on a web page.

```typescript
import { test, expect } from "@playwright/test";

test("Execute scripts inside iframe", async ({ page }) => {
  await page.goto("https://example.com");

  // Find frame by name
  const frame = page.frame({ name: "frameName" });
  if (frame) {
    // Execute script inside the frame
    const frameTitle = await frame.evaluate(() => document.title);
    console.log(`Title of the frame: ${frameTitle}`);

    // Set value in local storage inside the frame
    await frame.evaluate(() => localStorage.setItem("frameItem", "frameValue"));

    // Check value in local storage inside the frame
    const localStorageValue = await frame.evaluate(() =>
      localStorage.getItem("frameItem")
    );
    expect(localStorageValue).toBe("frameValue");
  }
});
```

## Elements and Selectors

### Finding and Interacting with Elements

Playwright allows you to find elements on the page using various selectors and interact with them. One of the key aspects of Playwright is that you do not need to explicitly wait for elements to appear on the page. All actions with elements automatically include waiting for the element to be available for interaction.

```typescript
import { test, expect } from "@playwright/test";

test("Element interaction test", async ({ page }) => {
  await page.goto("https://example.com");
  await page.click("text=Submit");
  await page.fill("#username", "myusername");
  await expect(page.locator("#username")).toHaveValue("myusername");
});
```

### Types of Selectors and Best Practices

Playwright supports several types of selectors:

- **CSS Selectors**: Using standard CSS selectors.
- **Text Selectors**: Finding elements by text.
- **XPath Selectors**: Using XPath expressions.

Examples of selectors:

```typescript
import { test, expect } from "@playwright/test";

test("Selector types test", async ({ page }) => {
  await page.goto("https://example.com");
  // Text selector
  await page.click("text=Login");
  // CSS selector
  await page.fill('input[name="username"]', "myusername");
  // XPath selector
  await page.click('//button[@id="submit"]');
});
```

### Interacting with Elements

Playwright allows you to perform various actions with elements, such as clicking, typing text, hovering the mouse, and much more.

```typescript
import { test, expect } from "@playwright/test";

test("Various interactions test", async ({ page }) => {
  await page.goto("https://example.com");
  await page.click("text=Submit");
  await page.fill("#username", "myusername");
  await page.hover("text=Hover me");
  await page.selectOption("select#options", "value1");
  await expect(page.locator("select#options")).toHaveValue("value1");
});
```

## Screenshots

Playwright allows you to take screenshots of the page or individual elements, which can be useful for visual testing and debugging.

### Full Page Screenshot

```typescript
import { test, expect } from "@playwright/test";

test("Take full page screenshot", async ({ page }) => {
  await page.goto("https://example.com");

  // Full page screenshot
  await page.screenshot({ path: "screenshot.png", fullPage: true });
});
```

### Element Screenshot

```typescript
import { test, expect } from "@playwright/test";

test("Take element screenshot", async ({ page }) => {
  await page.goto("https://example.com");

  // Screenshot of a specific element
  const element = await page.$("h1");
  await element?.screenshot({ path: "element.png" });
});
```

### Screenshot Testing

`toHaveScreenshot` is a method from the `@playwright/test` library that is used to create and compare screenshots in Playwright tests. This method allows you to automatically save and compare screenshots with reference images, simplifying visual testing.

```typescript
import { test, expect } from "@playwright/test";

test.describe("Screenshot Testing", () => {
  test("Take full page screenshot", async ({ page }) => {
    await page.goto("https://example.com");

    // Full page screenshot and compare with reference image
    await expect(page).toHaveScreenshot("fullpage.png", {
      fullPage: true,
    });
  });

  test("Take element screenshot", async ({ page }) => {
    await page.goto("https://example.com");

    // Screenshot of a specific element and compare with reference image
    const element = await page.locator("h1");
    await expect(element).toHaveScreenshot("element.png");
  });
});
```

#### Configuration

To work with the `toHaveScreenshot` methods, you may need to configure the Playwright configuration file `playwright.config.ts`:

```typescript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  testDir: "./tests",
  expect: {
    toHaveScreenshot: {
      threshold: 0.2, // Threshold value for differences
    },
  },
});
```

This method significantly simplifies the visual testing process by automatically checking that your application's interface has not changed.

## Intercepting and Modifying Network Requests

Playwright allows you to intercept and modify network requests, which can be useful for testing APIs and simulating various server states.

```typescript
import { test, expect } from "@playwright/test";

test("Intercept and modify network requests", async ({ page }) => {
  await page.route("**/api/*", (route) => {
    if (route.request().url().includes("/api/data")) {
      route.fulfill({
        status: 200,
        contentType: "application/json",
        body: JSON.stringify({ key: "value" }),
      });
    } else {
      route.continue();
    }
  });

  await page.goto("https://example.com");

  // Check modified response
  const response = await page.waitForResponse("**/api/data");
  const data = await response.json();
  expect(data.key).toBe("value");
});
```

## Recording Video Sessions

Playwright can record videos of page interactions, which is especially useful for debugging and analyzing tests.

```typescript
import { test, expect } from "@playwright/test";

test("Record video of the session", async ({ browser }) => {
  const context = await browser.newContext({ recordVideo: { dir: "videos/" } });
  const page = await context.newPage();

  await page.goto("https://example.com");
  await page.click("text=Submit");

  // Closing the context after the test will save the video
  await context.close();
});
```

## Snapshots

### Snapshot Testing in Playwright

Snapshot testing is a method of testing where the current state of the application (e.g., the DOM tree) is saved and compared with a previously saved reference (snapshot). This method helps to identify unintentional changes in the application's interface.

Playwright supports snapshot testing through the `toMatchSnapshot` method.

### Key Points for Using Snapshots

1. **Creating a Snapshot**: During the first run, Playwright creates a reference snapshot of the current state.
2. **Comparing Snapshots**: On subsequent runs, Playwright compares the current state with the reference snapshot.
3. **Updating Snapshots**: If changes are expected and correct, the reference snapshot can be updated.

### Examples of Using Snapshots

#### Comparing the DOM Tree

Comparing the current state of the DOM tree of a page with a reference snapshot.

```typescript
import { test, expect } from "@playwright/test";

test("DOM snapshot comparison", async ({ page }) => {
  await page.goto("https://example.com");

  // Save and compare DOM tree snapshot
  await expect(page).toMatchSnapshot("dom-snapshot.snapshot", {
    threshold: 0.1, // Threshold value for differences between snapshots
    maxDiffPixels: 50, // Maximum allowable number of differing pixels
  });
});
```

### Updating Snapshots

If changes in the interface are expected and correct, reference snapshots can be updated using the `--update-snapshots` flag when running tests.

```bash
npx playwright test --update-snapshots
```

### Snapshot Configuration

In the configuration file `playwright.config.ts`, you can set parameters for snapshot testing:

```typescript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  testDir: "./tests",
  expect: {
    toMatchSnapshot: {
      threshold: 0.2, // Threshold value for differences between snapshots
      maxDiffPixels: 100, // Maximum allowable number of differing pixels
    },
  },
});
```

### Conclusion

Snapshot testing in Playwright is a powerful tool for automatically testing changes and the state of the DOM tree. It simplifies the process of detecting regressions and maintaining the stability of your application's interface.

These unique features of Playwright make it a powerful tool for automating the testing of web applications, allowing you to flexibly and efficiently verify the functionality and performance of your applications.

These core concepts will help you understand how to use Playwright to automate interaction with web applications.

---

[Back: Installation and Setup](../sections/2_installation_and_setup.md)

[Next: Playwright UI](../sections/4_ui_mode.md)

[Contents](../sections.md)
