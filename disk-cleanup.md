Зупинка служб Windows Update
```
Stop-Service -Name wuauserv,bits -Force -ErrorAction SilentlyContinue
```

Очищення кешу Windows Update
```
Remove-Item -Path 'C:\Windows\SoftwareDistribution\Download\*' -Recurse -Force -ErrorAction SilentlyContinue
```

Запуск служб назад
```
Start-Service -Name bits -ErrorAction SilentlyContinue
Start-Service -Name wuauserv -ErrorAction SilentlyContinue
```

Очищення Component Store / WinSxS
```
Dism.exe /Online /Cleanup-Image /StartComponentCleanup
```
