# VBS2Function

VBS2Function is a PowerBASIC program that I developed to transform an existing VBSCript .VBS file to a TCC Function for use with the TCC @SCRIPT function.

The Help for the TCC @SCRIPT function states;

```dos
@SCRIPT[engine,expression] : Returns the integer result of expression in the specified active scripting engine.
```


In my case, I do not want the integer result of an expression, I want to display text on the screen.

WScript will not work with the @SCRIPT function, so I have to access STDOUT via VBScript.

If I want to start from scratch and create a new VBScript with a templete for STDOUT access, I can do the following;

```dos
e:\documents\powerbasic\vbs2function>vbs2function test.vbs
File test.vbs does not exist.
Creating a template file called test.vbs

The file test.vbs has been created. Use your editor to make modifications.
```

I then use my text editor to edit and add to the test.vbs file.

```dos
set fso=CreateObject("Scripting.FileSystemObject")
set stdout=fso.GetStandardStream(1)
stdout.WriteLine "Hi"
set fso=Nothing
'Created using vbs2Function.exe
```
I then run the same vbs2function line again;

```dos
e:\documents\powerbasic\vbs2function>vbs2function test.vbs
'Created using vbs2Function.exe
Your TCC Function is on the clipboard.
Press Shift-Insert To Paste it to the command line.
```
Next, I press Ctrl-V (seems to be faster then Shift-Insert);

```dos
e:\documents\powerbasic\vbs2function>function test=`%@script[vbscript,set fso=CreateObject("Scripting.FileSyste
mObject"):set stdout=fso.GetStandardStream(1):stdout.WriteLine "Hi":set fso=Nothing:]`
```
To confirm the newly created TCC Function;

```dos
e:\documents\powerbasic\vbs2function>function test
%@script[vbscript,set fso=CreateObject("Scripting.FileSystemObject"):set stdout=fso.GetStandardStream(1):stdout
.WriteLine "Hi":set fso=Nothing:]
```
Now I can use the newly created TCC Function;
```dos
e:\documents\powerbasic\vbs2function>echo %@test[] > nul
Hi
```
I could also use the TCC Script function to run test.vbs;
```dos
e:\documents\powerbasic\vbs2function>script test.vbs
Hi
```
...but that means I would have to capture the output myself, maybe using the TCC @EXECSTR function. Using @SCRIPT captures the output from test.vbs
