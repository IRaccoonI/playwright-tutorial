### Сравнение Selenium, Puppeteer, Cypress с Playwright

#### 1. **Поддержка браузеров**

- **Playwright**: Поддерживает Chromium, Firefox и WebKit, что делает его гибким и современным.
- **Puppeteer**: Поддерживает только Chromium-based браузеры (Electron, new Edge).
- **Cypress**: Поддерживает Chromium-based и Firefox, но ограниченная поддержка других браузеров.
- **Selenium**: Поддерживает больше браузеров (Chrome, Firefox, Edge, IE, Safari, Opera), но сложнее в настройке.

#### 2. **Поддержка языков программирования**

- **Playwright**: Поддерживает Java, Python, C#, JavaScript, TypeScript.
- **Puppeteer**: Только JavaScript и TypeScript.
- **Cypress**: Только JavaScript и TypeScript.
- **Selenium**: Поддерживает множество языков (Java, Python, C#, Ruby, JavaScript).

#### 3. **Мультиплатформенность**

- Все инструменты поддерживают Windows, macOS и Linux.

#### 4. **Скорость и стабильность**

- **Puppeteer**: Быстрее благодаря DevTools протоколу.
- **Playwright**: Быстрее и стабильнее благодаря DevTools протоколу для нескольких браузеров.
- **Cypress**: Высокая производительность для поддерживаемых браузеров.
- **Selenium**: Может быть медленным из-за использования WebDriver.

#### 5. **Особенности**

- **Playwright**: Современный, мультибраузерный, автоматическое ожидание элементов, поддержка мобильных устройств.
- **Selenium**: Наиболее зрелый и гибкий, но сложнее в настройке и медленнее.
- **Puppeteer**: Простота и высокая производительность для Chrome, но ограничена в браузерах.
- **Cypress**: Простота и высокая производительность для фронтенд-разработчиков, но ограничен в браузерах.

### Примеры кода с ожиданиями элементов

**Playwright (TypeScript)**:

```typescript
import { test, expect } from "@playwright/test";

test("example test with waiting", async ({ page }) => {
  await page.goto("https://example.com");

  // Проверка текста элемента
  expect(await heading.textContent()).toBe("Example Domain");
});
```

**Selenium (JavaScript)**:

```javascript
const { Builder, By, until } = require("selenium-webdriver");

(async function example() {
  let driver = await new Builder().forBrowser("firefox").build();
  try {
    await driver.get("https://example.com");

    // Ожидание видимости элемента
    let heading = await driver.wait(
      until.elementLocated(By.tagName("h1")),
      10000
    );
    await driver.wait(until.elementIsVisible(heading), 10000);

    // Проверка текста элемента
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

    // Проверка текста элемента
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

  // Ожидание видимости элемента
  await page.waitForSelector("h1", { visible: true });

  // Проверка текста элемента
  const heading = await page.$eval("h1", (el) => el.textContent);
  console.log(heading === "Example Domain");

  await browser.close();
})();
```

### Дополнительные статьи со сравнением,

- [общее](https://www.testim.io/blog/puppeteer-selenium-playwright-cypress-how-to-choose/)

- [производительности](https://www.checklyhq.com/blog/cypress-vs-selenium-vs-playwright-vs-puppeteer-speed-comparison/)

---

[Назад: Playwright UI](../sections/4_ui_mode.md)

[Далее: Продвинутые возможности](../sections/6_advanced_features.md)

[Содержание](../sections.md)
