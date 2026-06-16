Я скопіював із ZIP-профілю `191329782.datadir.zip` у `806093866.datadir.zip` такі частини Chromium/Dolphin профілю:

- `Default/Extensions` - самі файли встановлених розширень.
- `Default/Local Extension Settings` - локальні дані розширень.
- `Default/Sync Extension Settings` - sync-налаштування розширень, якщо були.
- `Default/Extension State` - службовий стан розширень.
- `Default/Extension Rules` - правила розширень.
- `Default/Extension Scripts` - скрипти розширень, якщо були.
- `Default/Extension Cookies` - cookies розширень.
- `Default/IndexedDB/chrome-extension_*` - IndexedDB-сховища розширень.
- Секцію `extensions` у `Default/Preferences` - щоб браузер бачив ці розширення як встановлені та з їхніми налаштуваннями.

Джерельний профіль `191329782` я не змінював. Перед зміною цільового профілю зробив backup ZIP у папці `806093866`.
