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
https://learn.microsoft.com/en-us/sysinternals/downloads/handle
### <u>Example Usage of mmfwrite.btm and mmfread.btm</u>
```dos
mmfopen.btm mmf1

mmfwrite.btm %_isodate

mmfread.btm
2023-06-03
```

```dos
R:\>type test.txt
06:36:38

R:\>mmfwrite.btm < R:\Test.txt

R:\>mmfread
06:36:38
```

```dos
R:\>echo %_time |! mmfwrite.btm

R:\>mmfread
07:49:41
```
## Scripts

### <u>[MMFOpen.btm][1]</u>

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

:: 0 = Re-direct
::
:: mmfwrite.btm < r:\test.txt
:: echo %_time |! mmfwrite.btm
::
:: 1 = Console
::
:: mmfwrite.btm This is a test.

iff %# eq 0 then
  iff %_stdin eq 1 then
    echo USAGE: %_batchname Text to store in MMF
    quit
  endiff
endiff

Gosub ClearMMF
on error goto errTrap
iff %_stdin eq 1 then
  iff %@smwrite[%mmf1,0,a,%$] ne 0 then
    echo Error writing to MMF
  endiff
else
  do line in @con:
    iff %@smwrite[%mmf1,0,a,%line] ne 0 then
      echo Error writing to MMF
    endiff
  enddo
endiff
endlocal
quit

:errTrap
::  6 The handle is invalid.
iff %_syserr eq 6 then
  echo %@errtext[6]
else
  echo %@errtext[%_syserr]
endiff
quit
Return

:ClearMMF
set MMF1Len=%@len[%{mmfread.btm}]
set MMF1Len=%@dec[%MMF1Len]
do kount=0 to %MMF1Len
  echo %@smpoke[%mmf1,%kount,4,0] > nul
enddo
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

  [1]: mmfopen.btm
