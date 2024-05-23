# Основные концепции

## Браузеры и контексты

### Поддерживаемые браузеры

Playwright поддерживает автоматизацию для следующих браузеров:

- Chromium
- Firefox
- WebKit

### Создание и управление контекстами

Контексты в Playwright позволяют создавать изолированные сессии браузера, что полезно для параллельного выполнения тестов и работы с различными состояниями. Пример создания контекста:

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

  // Проверка геолокации (возможно нужно больше логики для конкретного сайта)
  const geo = await page.evaluate(() => {
    return {
      latitude: navigator.geolocation.latitude,
      longitude: navigator.geolocation.longitude,
    };
  });
  expect(geo.latitude).toBe(37.7749);
  expect(geo.longitude).toBe(-122.4194);

  // Проверка Заголовков
  const response = await page.waitForResponse("https://example.com");
  const headers = await response.request().headers();
  expect(headers["authorization"]).toBe("Bearer my-token");

  await context.close();
});
```

## Страницы и фреймы

### Работа со страницами

Страница (`page`) — это основная сущность для взаимодействия с контентом веб-страницы. Вы можете открывать новые страницы, переходить по URL и взаимодействовать с элементами на странице.

```typescript
import { test, expect } from "@playwright/test";

test("Page navigation test", async ({ page }) => {
  await page.goto("https://example.com");
  const title = await page.title();
  expect(title).toBe("Example Domain");
});
```

### Навигация и управление фреймами(странно)

Фреймы позволяют работать с встраиваемыми элементами (iframe) на веб-странице.

```typescript
import { test, expect } from "@playwright/test";

test("Execute scripts inside iframe", async ({ page }) => {
  await page.goto("https://example.com");

  // Поиск фрейма по имени
  const frame = page.frame({ name: "frameName" });
  if (frame) {
    // Выполнение скрипта внутри фрейма
    const frameTitle = await frame.evaluate(() => document.title);
    console.log(`Title of the frame: ${frameTitle}`);

    // Установка значения в локальное хранилище внутри фрейма
    await frame.evaluate(() => localStorage.setItem("frameItem", "frameValue"));

    // Проверка значения в локальном хранилище внутри фрейма
    const localStorageValue = await frame.evaluate(() =>
      localStorage.getItem("frameItem")
    );
    expect(localStorageValue).toBe("frameValue");
  }
});
```

## Элементы и селекторы

### Поиск и взаимодействие с элементами

Playwright позволяет находить элементы на странице с помощью различных селекторов и взаимодействовать с ними. Один из ключевых аспектов Playwright заключается в том, что вам не нужно явно ожидать появления элементов на странице. Все действия с элементами автоматически включают ожидание, что элемент станет доступен для взаимодействия.

```typescript
import { test, expect } from "@playwright/test";

test("Element interaction test", async ({ page }) => {
  await page.goto("https://example.com");
  await page.click("text=Submit");
  await page.fill("#username", "myusername");
  await expect(page.locator("#username")).toHaveValue("myusername");
});
```

### Типы селекторов и лучшие практики

Playwright поддерживает несколько типов селекторов:

- **CSS селекторы**: Использование стандартных CSS селекторов.
- **Текстовые селекторы**: Поиск элементов по тексту.
- **XPath селекторы**: Использование XPath выражений..
- **React селекторы**: Поиск по имени компонента и пропам.
- **Vue селекторы**: Поиск по имени компонента и пропам.

Примеры селекторов:

```typescript
import { test, expect } from "@playwright/test";

test("Selector types test", async ({ page }) => {
  await page.goto("https://example.com");
  // Текстовый селектор
  await page.click("text=Login");
  // CSS селектор
  await page.fill('input[name="username"]', "myusername");
  // XPath селектор
  await page.click('//button[@id="submit"]');
});
```

### Взаимодействие с элементами

Playwright позволяет выполнять различные действия с элементами, такие как клики, ввод текста, наведение мыши и многое другое.

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

## Assertions

В Playwright тестовые объявления представлены в виде функции expect. Для выполнения объявления вызовите expect(значение) и выберите соответствующий матчер, отражающий ожидаемое состояние. Естьmany общих матчеров, такие как toEqual, toContain, toBeTruthy, которые можно использовать для утверждения любых условий.

```typescript
expect(success).toBeTruthy();
```

Playwright также включает в себя веб-специфические асинхронные матчеры, которые будут ожидать пока ожидаемое состояние не будет достигнуто. Рассмотрим пример:

```typescript
await expect(page.getByTestId("status")).toHaveText("Submitted");
```

Playwright будет повторно тестировать элемент с идентификатором test id 'status', пока элемент не будет иметь текст "Submitted". Он будет постоянно перезапрашивать элемент и проверять его, пока условие не будет выполнено или до тех пор, пока не истечет таймаут. Вы можете либо передавать этот таймаут, либо настроить его один раз через тестовый конфигурационный файл testConfig.expect.

По умолчанию таймаут для утверждений установлен на 5 секунд. Узнайте больше о различных таймаутах.

[Assertions Playwright документация](https://playwright.dev/docs/test-assertions)

## Скриншоты

Playwright позволяет делать скриншоты страницы или отдельных элементов, что может быть полезно для визуального тестирования и отладки.

### Скриншот всей страницы

```typescript
import { test, expect } from "@playwright/test";

test("Take full page screenshot", async ({ page }) => {
  await page.goto("https://example.com");

  // Скриншот всей страницы
  await page.screenshot({ path: "screenshot.png", fullPage: true });
});
```

### Скриншот отдельного элемента

```typescript
import { test, expect } from "@playwright/test";

test("Take element screenshot", async ({ page }) => {
  await page.goto("https://example.com");

  // Скриншот отдельного элемента
  const element = await page.$("h1");
  await element?.screenshot({ path: "element.png" });
});
```

### Тестирование скриншотами

toHaveScreenshot — это метод из библиотеки @playwright/test, который используется для создания и сравнения скриншотов в тестах Playwright. Этот метод позволяет автоматически сохранять и сравнивать скриншоты с эталонными изображениями, что упрощает визуальное тестирование.

```typescript
import { test, expect } from "@playwright/test";

test.describe("Screenshot Testing", () => {
  test("Take full page screenshot", async ({ page }) => {
    await page.goto("https://example.com");

    // Скриншот всей страницы и сравнение с эталонным изображением
    await expect(page).toHaveScreenshot("fullpage.png", {
      fullPage: true,
    });
  });

  test("Take element screenshot", async ({ page }) => {
    await page.goto("https://example.com");

    // Скриншот отдельного элемента и сравнение с эталонным изображением
    const element = await page.locator("h1");
    await expect(element).toHaveScreenshot("element.png");
  });
});
```

#### Конфигурация

Для работы с методами toHaveScreenshot может понадобиться настроить конфигурационный файл Playwright playwright.config.ts:

```typescript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  testDir: "./tests",
  expect: {
    toHaveScreenshot: {
      threshold: 0.2, // Пороговое значение для различий
    },
  },
});
```

Этот метод значительно упрощает процесс визуального тестирования, позволяя автоматически проверять, что интерфейс вашего приложения не изменился.

## Перехват и модификация сетевых запросов

Playwright позволяет перехватывать и модифицировать сетевые запросы, что может быть полезно для тестирования API и симуляции различных состояний сервера.

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

  // Проверка модифицированного ответа
  const response = await page.waitForResponse("**/api/data");
  const data = await response.json();
  expect(data.key).toBe("value");
});
```

## Запись видео сессии

Playwright может записывать видео взаимодействия с страницей, что особенно полезно для отладки и анализа тестов.

```typescript
import { test, expect } from "@playwright/test";

test("Record video of the session", async ({ browser }) => {
  const context = await browser.newContext({ recordVideo: { dir: "videos/" } });
  const page = await context.newPage();

  await page.goto("https://example.com");
  await page.click("text=Submit");

  // Закрытие контекста после выполнения теста сохранит видео
  await context.close();
});
```

## Снапшоты

### Snapshot Testing в Playwright

Snapshot testing (тестирование снапшотов) — это метод тестирования, при котором текущее состояние приложения (например, DOM-дерево) сохраняется и сравнивается с ранее сохраненным эталоном (снапшотом). Этот метод помогает выявлять непреднамеренные изменения в интерфейсе приложения.

Playwright поддерживает тестирование снапшотов через метод `toMatchSnapshot`.

### Основные моменты использования снапшотов

1. **Создание снапшота**: При первом запуске теста Playwright создает эталонный снапшот текущего состояния.
2. **Сравнение снапшотов**: При последующих запусках тестов Playwright сравнивает текущее состояние с эталонным снапшотом.
3. **Обновление снапшотов**: Если изменения ожидаемы и корректны, эталонный снапшот можно обновить.

### Примеры использования снапшотов

#### Сравнение DOM-дерева

Сравнение текущего состояния DOM-дерева страницы с эталонным снапшотом.

```typescript
import { test, expect } from "@playwright/test";

test("DOM snapshot comparison", async ({ page }) => {
  await page.goto("https://example.com");

  // Сохранение и сравнение снапшота DOM-дерева
  await expect(page).toMatchSnapshot("dom-snapshot.snapshot", {
    threshold: 0.1, // Пороговое значение для различий между снапшотами
    maxDiffPixels: 50, // Максимально допустимое количество различающихся пикселей
  });
});
```

### Обновление снапшотов

Если изменения в интерфейсе ожидаемы и корректны, эталонные снапшоты можно обновить. Для этого можно использовать флаг `--update-snapshots` при запуске тестов.

```bash
npx playwright test --update-snapshots
```

### Конфигурация снапшотов

В конфигурационном файле `playwright.config.ts` можно задать параметры для тестирования снапшотов:

```typescript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  testDir: "./tests",
  expect: {
    toMatchSnapshot: {
      threshold: 0.2, // Пороговое значение для различий между снапшотами
      maxDiffPixels: 100, // Максимально допустимое количество различающихся пикселей
    },
  },
});
```

### Заключение

Snapshot testing в Playwright — это мощный инструмент для автоматического тестирования изменений и состояния DOM-дерева. Он упрощает процесс обнаружения регрессий и поддержания стабильности интерфейса вашего приложения.

Эти уникальные возможности Playwright делают его мощным инструментом для автоматизации тестирования веб-приложений, позволяя гибко и эффективно проверять функциональность и производительность ваших приложений.

Эти основные концепции помогут вам понять, как использовать Playwright для автоматизации взаимодействия с веб-приложениями.

---

[Назад: Установка и настройка](../sections/2_installation_and_setup.md)

[Далее: Playwright UI](../sections/4_ui_mode.md)

[Содержание](../sections.md)
