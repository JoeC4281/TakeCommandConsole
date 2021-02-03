# SCRIPT and DynamicWrapperX
* Note that you will need to download and install a 64-bit COM DLL for this tip.
* Your anti-virus program will say that this is a virus/malware/destroyer of worlds, but it is not.
* I have tested this on my system;


Code:
```dos
     _x64: 1
   _admin: 1
_elevated: 1

TCC  27.00.18 x64   Windows 10 [Version 10.0.19042.746]
```
DynamicWrapperX allows you to call .DLLs from TCC (like [@WINAPI](https://jpsoft.com/help/f_winapi.htm) and [@CAPI](https://jpsoft.com/help/f_capi.htm)) using the [SCRIPT](https://jpsoft.com/help/script.htm) command, and you can also do low-level coding.

Here is an example of how I can use the DynamicWrapperX with the TCC SCRIPT command.

Save the following as test.js (or whatever.js), and then run it from the command line;

Code:
```dos
script test.js
```
Code:
```dos
DX = new ActiveXObject("DynamicWrapperX");
DX.Register("kernel32", "GetCommandLine", "r=s");      // This function has no parameters.
CmdLine = DX.GetCommandLine();                         // The command that started the process.
TakeCommand.tcommand("echo " + CmdLine);
```
You can even register a callback with the DynamicWrapperX.

The DynamicWrapperX can also be used from PowerShell;

Code:
```dos
PS E:\utils> $dx = CreateObject('DynamicWrapperX')
PS E:\utils> $dx.Register('kernel32', 'GetCommandLine', 'r=s')
2293006582496
PS E:\utils> $dx.GetCommandLine()
Powershell.exe
PS E:\utils>
```
It will also work with TCC v27 PSHELL;

Code:
```dos
e:\utils>pshell /s "$dx = New-Object -com DynamicWrapperX"

e:\utils>pshell /s "$dx.Register('kernel32', 'GetCommandLine', 'r=s')"
1969015197280

e:\utils>pshell /s "$dx.GetCommandLine()"
"C:\Program Files\JPSoft\TCMD26\tcc.exe"
```
Here is an example of how to do low-level coding;

Code:
```dos
DWX = new ActiveXObject("DynamicWrapperX");
if (DWX.Bitness == 32) {
    // The function multiplies its arguments and returns the result.
    Code = "8B442404F76C2408C3"
}
else {
    Code = "4889C8 48F7EA C3" // mov rax,rcx; imul rdx; ret
}
CodeAddr = DWX.RegisterCode(Code, "Multiply", "i=ll", "r=l");

TakeCommand.tcommand("echo " + DWX.Multiply(5, 4));
```
Save this as test.js, and then run it from the command line;

Code:
```dos
script test.js
```
which returns;

Code:
```dos
e:\utils>script test.js
20
```
Detailed info about DynamicWrapperX can be found at the following websites;

Link to DynamicWrapperX (Version 2)
Ref: [DynamicWrapperX Home Page](https://dynwrapx.script-coding.com/dwx/pages/dynwrapx.php?lang=en)

Link to DynamicWrapperX (Version 1)
Ref: [DynamicWrapperX Help](https://www.script-coding.com/dynwrapx_eng.html)
