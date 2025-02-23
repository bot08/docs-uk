# Розгортання продакшну {#production-deployment}

## Розробка проти продакшну {#development-vs-production}

Під час розробки Vue надає ряд функцій для покращення зручності розробки:

- Попередження про типові помилки та підводні камені
- Перевірка реквізитів / подій
- [Хуки налагодження реактивності](/guide/extras/reactivity-in-depth.html#reactivity-debugging)
- Інтеграція Devtools

Однак ці функції стають марними у продакшні. Деякі попередження перевірок, також можуть додавати невеликі витрати до продуктивності. Під час розгортання продакшну, ми повинні відмовитися від усіх невикористаних гілок коду, призначених лише для розробки, для зменшення розміру корисного навантаження та кращої продуктивності.

## Без інструментів збірки {#without-build-tools}

Якщо ви використовуєте Vue без інструмента збірки, завантажуючи його з CDN, або самостійно розміщеного сценарію, переконайтеся, що використовуєте продакшн збірку (файли dist, які закінчуються на `.prod.js`) під час розгортання продакшну. Продакшн збірки попередньо зменшені після видалення усіх гілок коду, які використовуються лише для розробки.

- Якщо використовується глобальна збірка (глобальний доступ до `Vue`): використовуйте `vue.global.prod.js`.
- Якщо використовується ESM збірка (доступ через власні ESM імпорти): використовуйте `vue.esm-browser.prod.js`.

Зверніться до [гіда](https://github.com/vuejs/core/tree/main/packages/vue#which-dist-file-to-use), щоб дізнатися більше.

## З інструментами збірки {#with-build-tools}

Проєкти, створені за допомогою `create-vue` (на основі Vite), або Vue CLI (на основі webpack) попередньо налаштовані для продакшн збірок.

Якщо ви використовуєте користувацьке налаштування, переконайтеся, що:

1. `vue` перетворюється на `vue.runtime.esm-bundler.js`.
2. [особливі прапорці під час компіляції](https://github.com/vuejs/core/tree/main/packages/vue#bundler-build-feature-flags) правильно налаштовані.
3. <code>process.env<wbr>.NODE_ENV</code> замінюється на `"production"` під час збірки.

Додаткові посилання:

- [Vite гід для продакшн збірок](https://vitejs.dev/guide/build.html)
- [Vite гід для розгортання ](https://vitejs.dev/guide/static-deploy.html)
- [Vue CLI гід для розгортання](https://cli.vuejs.org/guide/deployment.html)

## Відстеження помилок під час виконання {#tracking-runtime-errors}

[Глобальний обробник помилок](/api/application.html#app-config-errorhandler) можна використовувати для повідомлення служб відстеження про помилки:

```js
import { createApp } from 'vue'

const app = createApp(...)

app.config.errorHandler = (err, instance, info) => {
  // повідомлення служб відстеження про помилки
}
```

Такі служби, як [Sentry](https://docs.sentry.io/platforms/javascript/guides/vue/) і [Bugsnag](https://docs.bugsnag.com/platforms/javascript/vue/) також надають офіційну інтеграцію з Vue.
