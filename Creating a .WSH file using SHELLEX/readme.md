PROPS is a command from the SYSUTILS plugin, available from;

[Take Command Plugins for TCC and TCC/LE Command Prompts](https://jpsoft.com/all-downloads/plugins-take-command.html)

I am using the 64-bit version of SYSUTILS, as I am using the 64-bit version of TCC.

From the SYSUTILS.TXT file;

```dos
PROPS

PROPS object ... open the objects properties dialog
```
Here is how to create, from TCC, a .WSH file using PROPS;

```dos
PROPS test.vbs
```

From the context menu, Script tab;

![image](https://user-images.githubusercontent.com/58880711/137628293-84906bfd-7345-4f28-8cb9-907ff07b6f4c.png)

I can make the changes that I want, click Apply (or OK), and the test.wsh file is created for me, thus; 

```dos
e:\utils>type test.wsh
[ScriptFile]
Path=E:\Utils\test.vbs
[Options]
Timeout=1
DisplayLogo=0
```

Ref: https://jpsoft.com/forums/threads/creating-a-wsh-file-using-shellex.10420/#post-58664
