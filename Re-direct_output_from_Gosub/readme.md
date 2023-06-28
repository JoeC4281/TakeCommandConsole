I was not aware that one could re-direct output from Gosub, but apparently it can be done;

```dos
@setlocal
@echo off
gosub :writefile >output.txt
type output.txt
del /q output.txt
endlocal
quit

:writefile
echo _4ver   : %_4ver
echo _x64    : %_x64
echo _isodate: %_isodate
return
```

Ref: https://jpsoft.com/forums/threads/re-direct-output-from-gosub.10402/
