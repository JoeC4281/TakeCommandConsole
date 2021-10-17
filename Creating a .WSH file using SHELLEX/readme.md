SHELLEX is a command from the SYSUTILS plugin, available from;

[Take Command Plugins for TCC and TCC/LE Command Prompts](https://jpsoft.com/all-downloads/plugins-take-command.html)

I am using the 64-bit version of SYSUTILS, as I am using the 64-bit version of TCC.

From the SYSUTILS.TXT file;

```dos
SHELLEX

ShellExecute with registered or context menu verb

SHELLEX [/C] [/V verb] <file> [<arguments> [<directory>]]

  /C          use context menu

  /V verb     "print", "edit", "explore" et c.
              (with /C) "properties", "preview", et c.  Without "/V verb",
              the default verb is used if available, else "open" is used.

  /D          (debug) show parameter parsing without executing

  <arguments> and <directory> apply to launching executables

  When appropriate, <file> may specify a URL, a directory or a drive
  
  Quote parameters containing whitespace; escape necessary quotes as \"
```
Here is how to create, from TCC, a .WSH file using SHELLEX; 

From the context menu, Script tab;



Ref: https://jpsoft.com/forums/threads/creating-a-wsh-file-using-shellex.10420/#post-58664
