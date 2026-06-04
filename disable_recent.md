# Вимкнення Windows Recent Documents вручну через PowerShell

Ця інструкція показує, як вручну вимкнути збереження та показ історії **Recent Documents / Нещодавні документи** для поточного користувача Windows.

Зміни застосовуються тільки до поточного користувача, бо використовується гілка реєстру:

```powershell
HKCU
```

`HKCU` означає **HKEY_CURRENT_USER**.

---

## 1. Відкрий PowerShell

1. Натисни **Start**.
2. Введи **PowerShell**.
3. Відкрий **Windows PowerShell**.

Адміністраторські права зазвичай не потрібні, бо зміни робляться тільки для поточного користувача.

---

## 2. Створи потрібну гілку реєстру

Введи команду:

```powershell
New-Item -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer' -Force
```

Ця команда створює розділ реєстру `Explorer`, якщо його ще немає.

---

## 3. Вимкни збереження історії Recent Documents

Введи команду:

```powershell
New-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer' -Name 'NoRecentDocsHistory' -PropertyType DWord -Value 1 -Force
```

Що це робить:

- вимикає збереження історії нещодавніх документів;
- діє для поточного користувача Windows.

---

## 4. Вимкни показ нещодавніх документів у Start / Jump Lists

Введи команду:

```powershell
New-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced' -Name 'Start_TrackDocs' -PropertyType DWord -Value 0 -Force
```

Що це робить:

- вимикає відстеження нещодавно відкритих документів;
- може впливати на меню **Start**, **Jump Lists** і частково на поведінку **File Explorer**.

---

## 5. Вимкни додавання мережевих шляхів із Recent Documents

Введи команду:

```powershell
New-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer' -Name 'NoRecentDocsNetHood' -PropertyType DWord -Value 1 -Force
```

Що це робить:

- запобігає додаванню мережевих розташувань із нещодавно відкритих документів до Network Places.

---

## 6. Перевір, що значення записались

Перевір перші два параметри:

```powershell
Get-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer' -Name 'NoRecentDocsHistory','NoRecentDocsNetHood'
```

Перевір параметр `Start_TrackDocs`:

```powershell
Get-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced' -Name 'Start_TrackDocs'
```

Очікувані значення:

```text
NoRecentDocsHistory : 1
NoRecentDocsNetHood : 1
Start_TrackDocs     : 0
```

---

## 7. Перезапусти Explorer

Щоб зміни застосувалися швидше, введи:

```powershell
Stop-Process -Name explorer -Force
```

Після цього панель задач і вікна Провідника можуть зникнути на кілька секунд, а потім запуститися знову.

Альтернативний варіант: вийди з облікового запису Windows і зайди знову або перезавантаж комп’ютер.

---

# Як повернути все назад

Якщо потрібно знову дозволити історію Recent Documents, введи ці команди.

## 1. Видали політики вимкнення історії

```powershell
Remove-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer' -Name 'NoRecentDocsHistory' -ErrorAction SilentlyContinue
Remove-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer' -Name 'NoRecentDocsNetHood' -ErrorAction SilentlyContinue
```

## 2. Увімкни відстеження документів назад

```powershell
New-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced' -Name 'Start_TrackDocs' -PropertyType DWord -Value 1 -Force
```

## 3. Перезапусти Explorer

```powershell
Stop-Process -Name explorer -Force
```

---

# Важливі примітки

Ці команди не обов’язково видаляють уже наявну стару історію. Вони переважно вимикають подальше збереження та показ нещодавніх документів.

Також ці налаштування не очищають історію всередині окремих програм. Наприклад, Microsoft Word, Excel, браузери, редактори коду або медіаплеєри можуть мати власні списки нещодавно відкритих файлів.

У цих командах немає передачі даних в інтернет, читання приватних файлів або роботи з паролями чи ключами. Вони тільки змінюють налаштування реєстру Windows для поточного користувача.
