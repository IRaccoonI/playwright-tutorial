# Установка и настройка

## Установка Playwright

### Требования

Для работы с Playwright необходимо установить Node.js. Убедитесь, что у вас установлена последняя версия Node.js, которую можно загрузить с [официального сайта Node.js](https://nodejs.org/).

### Установка Playwright

Для установки Playwright в ваш проект выполните следующие шаги:

#### Установите Playwright с помощью npm:

```bash
npm install playwright
npx playwright install
```

Playwright также предоставляет возможность быстрой установки через команду `init`, которая автоматически настроит проект, эта команда установит все необходимые браузеры и создаст структуру проекта.

## Настройка проекта

### Конфигурационные файлы

Playwright позволяет настроить проект с помощью конфигурационного файла `playwright.config.js`. Этот файл используется для настройки параметров запуска тестов, браузеров и параллельного выполнения. Пример базовой конфигурации:

```javascript
const { devices } = require("@playwright/test");

module.exports = {
  testDir: "./tests", // Директория с тестами
  timeout: 30000, // Таймаут для каждого теста
  retries: 2, // Количество повторных запусков при неудаче
  outputDir: "./test-results", // Директория для сохранения результатов тестов
  workers: 4, // Количество параллельно выполняемых воркеров
  projects: [
    {
      name: "Chromium",
      use: {
        browserName: "chromium",
        headless: true, // Запуск тестов в headless режиме
        viewport: { width: 1280, height: 720 }, // Размер окна браузера
      },
    },
    {
      name: "Firefox",
      use: {
        browserName: "firefox",
        headless: false, // Запуск тестов в оконном режиме
      },
    },
    {
      name: "WebKit",
      use: {
        browserName: "webkit",
        ignoreHTTPSErrors: true, // Игнорирование ошибок HTTPS
      },
    },
    {
      name: "Mobile Safari",
      use: devices["iPhone 12"], // Использование настроек для мобильного устройства
    },
  ],
  use: {
    baseURL: "https://example.com", // Базовый URL для тестов
    headless: true, // Запуск тестов в headless режиме
    viewport: { width: 1280, height: 720 }, // Размер окна браузера
    ignoreHTTPSErrors: true, // Игнорирование ошибок HTTPS
    video: "retain-on-failure", // Запись видео только при неудачных тестах
    screenshot: "only-on-failure", // Захват скриншотов только при неудачных тестах
    trace: "on-first-retry", // Запись трассировки при первом повторном запуске
  },
  reporter: [
    ["list"], // Стандартный вывод в консоль
    ["json", { outputFile: "test-results/results.json" }], // JSON репортер
    ["html", { open: "never" }], // HTML репортер
    ["junit", { outputFile: "test-results/results.xml" }], // JUnit репортер
    ["line"], // Линейный вывод в консоль
    ["allure-playwright"], // Интеграция с Allure отчетностью
  ], // Конфигурация репортеров
  webServer: {
    command: "npm run start", // Команда для запуска веб-сервера
    url: "http://0.0.0.0:3000", // Адрес веб-сервера, для healthCheck
    timeout: 120 * 1000, // Таймаут ожидания запуска сервера
    reuseExistingServer: !process.env.CI, // Использование существующего сервера если тот существует, иначе запуск
  },
  expect: {
    timeout: 5000, // Таймаут для ожиданий expect в миллисекундах
    toHaveScreenshot: {
      // Параметры для сравнения скриншотов
      threshold: 0.2, // Пороговое значение для различий между скриншотами
      animations: "disabled", // Отключение анимаций для стабильных скриншотов
      scale: "css", // Масштабирование изображения в соответствии с CSS
    },
    toMatchSnapshot: {
      // Параметры для сравнения снапшотов
      threshold: 0.2, // Пороговое значение для различий между снапшотами
      maxDiffPixels: 100, // Максимально допустимое количество различающихся пикселей
    },
  },
};
```

- [Playwright configuration docs](https://playwright.dev/docs/test-configuration)
- [allure-playwright](https://allurereport.org/docs/playwright/)
- [junit фоточки](../assets/junit.md)

### Тестирование мобильных устройств в Playwright

Playwright поддерживает эмуляцию мобильных устройств, что позволяет тестировать веб-приложения на различных устройствах с разными размерами экранов и характеристиками. Это делает Playwright универсальным инструментом для E2E тестирования, охватывающим как десктопные, так и мобильные платформы.

#### Основные возможности тестирования мобильных устройств в Playwright

1. **Эмуляция мобильных устройств**: Playwright позволяет эмулировать популярные мобильные устройства, такие как iPhone и Android устройства, с учетом их экранных размеров, пользовательских агентов и других характеристик.
2. **Тестирование в реальных браузерах**: Эмуляция выполняется в реальных браузерах (Chromium, Firefox, WebKit), что обеспечивает высокую точность тестов.
3. **Геолокация и сенсоры**: Поддержка эмуляции геолокации и других сенсоров, таких как акселерометр.

#### Пример эмуляции мобильного устройства в Playwright

```typescript
import { test, devices } from "@playwright/test";

const iPhone11 = devices["iPhone 11"];

test.use({ ...iPhone11 });

test("Test on iPhone 11", async ({ page }) => {
  await page.goto("https://example.com");
  // Ваши тесты здесь
});
```

#### Отличия от тестирования в десктопных браузерах

1. **Размер экрана и разрешение**: Мобильные устройства имеют меньшие экраны и могут иметь разные разрешения, что влияет на отображение контента.
2. **Пользовательские агенты**: Мобильные браузеры используют разные пользовательские агенты, что может повлиять на поведение веб-страниц.
3. **Сенсоры и ориентация**: Возможность тестирования взаимодействия с сенсорами, такими как акселерометр и геолокация, которые не имеют аналогов в десктопных браузерах.

### Создание структуры проекта

Создайте следующую структуру каталогов для хранения тестов и конфигураций:

```
my-playwright-project/
├── tests/
│   └── example.spec.js
├── node_modules/
├── package.json
├── playwright.config.js
└── README.md
```

### Пример теста

Создайте файл `example.spec.js` в директории `tests` и добавьте следующий пример теста:

```javascript
const { test, expect } = require("@playwright/test");

test("basic test", async ({ page }) => {
  await page.goto("https://example.com");
  const title = await page.title();
  expect(title).toBe("Example Domain");
});
```

### Запуск тестов

Для запуска тестов используйте команду:

```bash
npx playwright test
```

Эта команда найдет все тесты в директории `tests` и выполнит их.

---

[Назад: Введение](../sections/1_introduction.md)

[Далее: Основные концепции](../sections/3_core_concepts.md)

[Содержание](../sections.md)
