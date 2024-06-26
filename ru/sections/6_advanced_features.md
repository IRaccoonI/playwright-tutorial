# Продвинутые возможности

## UI Mode в Playwright

UI Mode в Playwright — это инструмент, который предоставляет графический интерфейс для выполнения и отладки тестов. Он позволяет визуально наблюдать за выполнением тестов, анализировать результаты и взаимодействовать с браузером в реальном времени. Этот режим особенно полезен для разработчиков, так как он упрощает процесс отладки и тестирования.

### Основные возможности UI Mode

1. **Визуализация тестов**: Вы можете наблюдать за выполнением тестов в реальном времени, видя, как браузер выполняет каждое действие.
2. **Интерактивная отладка**: Возможность приостанавливать тесты, взаимодействовать с браузером и проверять состояние элементов на странице.
3. **Анализ результатов**: В UI Mode можно просматривать результаты тестов, включая ошибки, скриншоты и трассировки.

### Запуск UI Mode

Для запуска UI Mode используйте команду:

```bash
npx playwright test --ui
```

Эта команда откроет интерфейс UI Mode в браузере, где вы сможете выбирать тесты для выполнения и анализировать их результаты.

### Основные компоненты UI Mode

1. **Список тестов**: Отображает все тесты, доступные в вашем проекте. Вы можете запускать отдельные тесты или наборы тестов.
2. **Консоль и терминал**: Встроенная консоль отображает логи и результаты выполнения тестов.
3. **Скриншоты и видео**: Позволяет просматривать скриншоты и видео, записанные во время выполнения тестов.
4. **Трассировки**: Визуализация трассировок помогает анализировать шаги выполнения тестов, сетевые запросы и взаимодействия с элементами.

### Пример использования UI Mode

1. Откройте терминал и перейдите в директорию вашего проекта Playwright.
2. Запустите UI Mode командой `npx playwright test --ui`.
3. В открывшемся интерфейсе выберите тесты, которые вы хотите выполнить.
4. Наблюдайте за выполнением тестов в реальном времени. Вы можете приостанавливать тесты и взаимодействовать с браузером для отладки.

### Преимущества использования UI Mode

- **Удобство отладки**: Визуальный интерфейс позволяет быстрее и эффективнее выявлять проблемы в тестах.
- **Подробные отчеты**: Возможность просматривать скриншоты, видео и трассировки облегчает анализ результатов тестов.
- **Интерактивность**: Возможность приостанавливать тесты и взаимодействовать с браузером упрощает процесс отладки и тестирования.

[Документация Playwright по UI Mode](https://playwright.dev/docs/test-ui-mode)

## Дебаггинг в Playwright

Playwright предоставляет несколько мощных инструментов для отладки тестов, включая интерактивный режим `--debug` и интеграцию с Visual Studio Code (VS Code).

### Дебаггинг через команду `--debug`

Режим отладки Playwright можно запустить с помощью команды `--debug`. Это позволяет приостанавливать выполнение тестов, открывать DevTools и взаимодействовать с браузером вручную.

#### Запуск тестов в режиме отладки

Запустите тесты с флагом `--debug`:

```bash
npx playwright test --debug
```

После запуска этой команды тесты будут приостановлены перед выполнением, и откроется окно браузера с включенными DevTools. Вы можете пошагово выполнять тесты, ставить точки останова и анализировать текущее состояние браузера.

#### Использование `page.pause()`

Вы также можете добавить метод `page.pause()` в тест, чтобы приостановить выполнение в определенной точке:

```typescript
import { test, expect } from "@playwright/test";

test("Pause example", async ({ page }) => {
  await page.goto("https://example.com");
  await page.pause(); // Приостановка выполнения теста и открытие DevTools
  const title = await page.title();
  expect(title).toBe("Example Domain");
});
```

### Дебаггинг через Visual Studio Code (VS Code)

Playwright предлагает интеграцию с VS Code, что позволяет удобно отлаживать тесты прямо в редакторе.

#### Установка расширения Playwright для VS Code

1. Откройте VS Code.
2. Перейдите на вкладку Extensions (расширения) или нажмите `Ctrl+Shift+X`.
3. Найдите и установите расширение **Playwright Test for VSCode**.

#### Дополнительные возможности

- **Встроенные переменные окружения**: Вы можете настроить переменные окружения в конфигурации запуска для различных сред.
- **Консоль и терминал**: Вывод отладочной информации отображается в интегрированной консоли или терминале VS Code, что облегчает анализ логов и ошибок.

## Шардинг (Sharding) в Playwright

Шардинг (Sharding) — это метод разделения набора тестов на несколько частей (шардов), которые могут выполняться параллельно на разных машинах или процессах. Это позволяет значительно сократить общее время выполнения тестов и повысить производительность тестирования.

### Пример использования шардинга

Чтобы разделить тесты на несколько шардов, используйте параметр `--shard=x/y` в командной строке. Например, для разделения тестов на четыре шарда, каждый из которых будет выполнять одну четверть тестов:

```bash
npx playwright test --shard=1/4
npx playwright test --shard=2/4
npx playwright test --shard=3/4
npx playwright test --shard=4/4
```

Запустив эти шарды параллельно на разных машинах, вы сможете завершить выполнение набора тестов в четыре раза быстрее.

### Конфигурация шардинга

В конфигурационном файле `playwright.config.ts` можно задать параметры шардинга:

```typescript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  testDir: "./tests", // Директория с тестами
  retries: 2, // Количество повторных запусков при неудаче
  workers: 4, // Количество параллельно выполняемых воркеров
  shard: {
    total: 2, // Общее количество шардов
    current: 1, // Номер текущего шарда (начинается с 1)
  },
  use: {
    headless: true, // Запуск тестов в headless режиме
    viewport: { width: 1280, height: 720 }, // Размер окна браузера
    screenshot: "only-on-failure", // Захват скриншотов только при неудачных тестах
    trace: "on-first-retry", // Запись трассировки при первом повторном запуске
  },
});
```

### Слияние отчетов с нескольких шардов

Каждый шард тестов имеет свой собственный отчет. Чтобы получить объединенный отчет, показывающий результаты всех шардов, можно их объединить. Для этого добавьте `blob` репортер в конфигурацию:

Чтобы объединить отчеты с нескольких шардов, соберите файлы отчетов в одну директорию, например `all-blob-reports`, а затем выполните команду `npx playwright merge-reports`:

```bash
npx playwright merge-reports --reporter=html ./all-blob-reports
```

Это создаст стандартный HTML отчет в директории `playwright-report`.

[документации Playwright по шардингу](https://playwright.dev/docs/test-sharding)

---

[Назад: Сравнение с другими инструментами](../sections/5_comparing.md)

[Далее: Мб полезное](../sections/7_maybe_usefull.md)

[Содержание](../sections.md)
