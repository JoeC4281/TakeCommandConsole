# Using Memory Mapped Files with Take Command Console

## Tools

### <u>MMFDumper.exe</u>
MMFDumper is a command line utility that can be used to create a hex/text dump of a memory mapped file.

Example:
```dos
MMFDumper.exe MMF1 /maxlength=0
```


https://github.com/thoblerone/MMFDumper/blob/master/MMFDumper/bin/Release/MMFDumper.exe

### <u>HANDLE.EXE</u>
I use this to discover Memory Mapped Files that I create.

Example:
```dos
handle mmf -nobanner -p tcc
```

## Scripts

### <u>MMFOpen.btm</u>

```dos
@setlocal
@echo off
::
:: Determine if a handle to shared memory is open
::
iff defined mmf1 then
  set mmf*
  echo MMF1 is already open
  ::
  :: handle.exe is from https://learn.microsoft.com/en-us/sysinternals/downloads/handle
  ::
  handle.exe mmf1 -nobanner
  quit
endiff
set mmf1=%@smopen[10240,MMF1]
set mmf*
endlocal mmf1
```

### <u>MMFWrite.btm</u>
```dos
@setlocal
@echo off
iff defined mmf1 then
  :: echo mmf1 Exists
else
  echo mmf1 does not exist
  quit
endiff

iff %# eq 0 then
  echo USAGE: %_batchname Text to store in MMF
  quit
endiff
on error goto errTrap
:: echo %@smwrite[%mmf1,0,a,%$]
iff %@smwrite[%mmf1,0,a,%$] ne 0 then
  echo Error writing to MMF
endiff
endlocal
quit

:errTrap
::  6 The handle is invalid.
if %_syserr eq 6 echo %@errtext[6]
endiff
quit
Return
```
### <u>MMFRead.btm</u>
```dos
@setlocal
@echo off
on error goto errTrap
echo %@smread[%mmf1,0,a,10240]
endlocal
quit

:errTrap
echo %_syserr
:: 87 The parameter is incorrect.
quit
Return
```

### POKEing an 'A' into a Memory Mapped File
```dos
:: A is ASCII code 65
echo %@smpoke[%mmf1,0,1,65] > nul
```
