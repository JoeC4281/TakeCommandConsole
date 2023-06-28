# Avoid Continued Lines in your Source Code

```dos
@setlocal
@echo off

COMMENT
     _x64: 1
   _admin: 1
_elevated: 1

TCC  30.00.18 x64   Windows 10 [Version 10.0.19044.2965]
ENDCOMMENT

set _a=10
set _b=20
::
:: Set _test as an equation
::
set _test=%%_a eq 10 .or. %%_b eq 10
echo Does %_test?
if (%_test) (echo Yes) else (echo No)
::
:: Adjust values of _a and _b
::
set _a=11
set _b=21
echo Does %_test?
if (%_test) (echo Yes) else (echo No)
endlocal
```
