# 🎄 Новогодний вишлист

Одностраничник с двумя досками стикеров: Олег и Оля. Весь сайт — один файл `index.html`.

Без настройки желания хранятся только в браузере, где их добавили. Чтобы у вас двоих был общий список, который обновляется сразу на обоих устройствах, подключите бесплатную базу Firebase (5 минут).

## Подключение синхронизации

1. Откройте https://console.firebase.google.com → **Create a project**. Название любое, Google Analytics можно выключить.
2. В меню слева **Build → Realtime Database → Create database**. Регион — `europe-west1`, режим — **Start in locked mode**.
3. Вкладка **Rules** → замените всё на текст ниже → **Publish**:

   ```json
   {
     "rules": {
       "wishlist": {
         "items": {
           ".read": true,
           "$id": {
             ".write": true,
             ".validate": "newData.hasChildren(['owner', 'title']) && (newData.child('owner').val() == 'oleg' || newData.child('owner').val() == 'olya') && newData.child('title').isString() && newData.child('title').val().length <= 120"
           }
         }
       }
     }
   }
   ```

4. Шестерёнка → **Project settings** → внизу **Your apps** → иконка `</>` (Web). Имя любое, Firebase Hosting не нужен. Скопируйте объект `firebaseConfig`.
5. В `index.html` найдите строку `const FIREBASE_CONFIG = null;` и замените `null` на скопированный объект:

   ```js
   const FIREBASE_CONFIG = {
     apiKey: "…",
     authDomain: "…",
     databaseURL: "https://….europe-west1.firebasedatabase.app",
     projectId: "…",
     // остальные поля тоже можно оставить
   };
   ```

   Важно, чтобы в объекте было поле `databaseURL`. Если его нет — скопируйте адрес базы со страницы Realtime Database.

6. Под заголовком сайта появится зелёная точка «Синхронизировано». Желания, добавленные до подключения, загрузятся в базу автоматически.

## Публикация на GitHub Pages

Репозиторий на GitHub → **Settings → Pages** → Source: **Deploy from a branch**, ветка `main`, папка `/ (root)` → Save. Через минуту сайт будет доступен по адресу `https://<ваш-логин>.github.io/wishlist/`.

## Про доступ

Пароля нет: редактировать список может любой, кто знает адрес сайта. Для вишлиста на двоих этого обычно достаточно. Ключи в `firebaseConfig` не секретные — доступ ограничивают правила из шага 3.
