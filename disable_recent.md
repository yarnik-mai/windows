# Вимкнення Recent Documents

## Через Settings — найпростіший спосіб

```text
Settings → Personalization → Start → Show recommended files in Start, recent files in File Explorer, and items in Jump Lists
```

Після цього бажано ще очистити стару історію. Можна через PowerShell:

```powershell
Remove-Item "$env:APPDATA\Microsoft\Windows\Recent\*" -Force -Recurse -ErrorAction SilentlyContinue
Remove-Item "$env:APPDATA\Microsoft\Windows\Recent\AutomaticDestinations\*" -Force -ErrorAction SilentlyContinue
Remove-Item "$env:APPDATA\Microsoft\Windows\Recent\CustomDestinations\*" -Force -ErrorAction SilentlyContinue
Stop-Process -Name explorer -Force
```
