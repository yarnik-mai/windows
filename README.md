Вставлення
```
winget install --id OpenAI.Codex
```

Промт
```
Привіт. Треба діагностувати лаги Windows.
Запусти Procmon64 на 30 секунд, у цей час я буду відтворювати дію, яка лагає. 
Ціль: знайти не загальні припущення, а конкретний процес/файл/реєстр/мережевий шлях, на якому виникає затримка.
Симтоми: _
```

Автозапуск
```
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\Disabled
```

Змінити Стек
```
Start-Process powershell.exe -Verb RunAs

$b=[IO.File]::ReadAllBytes('C:\Program Files\Adobe\Adobe Photoshop 2025\Photoshop.exe'); $p=[BitConverter]::ToInt32($b,60); "Ліміт стека: $([BitConverter]::ToUInt64($b,($p+96))/1MB) МіБ"

& { $ErrorActionPreference='Stop'; $mb=16; $p='C:\Program Files\Adobe\Adobe Photoshop 2025\Photoshop.exe'; if(Get-Process Photoshop -ErrorAction SilentlyContinue){throw 'Закрий Photoshop'}; if($mb -lt 1 -or $mb -gt 64){throw 'Вкажи від 1 до 64 МіБ'}; Copy-Item -LiteralPath $p -Destination ($p+'.'+[guid]::NewGuid().ToString()+'.bak'); $s=[IO.File]::Open($p,'Open','ReadWrite','None'); $r=[IO.BinaryReader]::new($s); try{$s.Position=60; $pe=$r.ReadInt32(); $s.Position=$pe; if($r.ReadUInt32() -ne 17744){throw 'Невірний PE'}; $s.Position=$pe+24; if($r.ReadUInt16() -ne 523){throw 'Потрібен 64-бітний EXE'}; $s.Position=$pe+96; $b=[BitConverter]::GetBytes([uint64]($mb*1MB)); $s.Write($b,0,8); $s.Flush($true); $s.Position=$pe+96; "Ліміт стека: $($r.ReadUInt64()/1MB) МіБ"}finally{$r.Dispose(); $s.Dispose()} }
```
