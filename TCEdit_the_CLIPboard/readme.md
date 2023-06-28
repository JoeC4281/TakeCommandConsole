# TCEdit the CLIPboard

While I have been using TCEDIT for making changes to the **%_ininame** and the **%_tcstart** files, and as a STDOUT editor (myfile.btm | TCEDIT) I just discovered that I can use TCEDIT to make changes to the clipboard; 

```dos
tcedit clip:
```
Once I have made the changes to the clipboard, I do a Ctrl+A (select all) followed by a Ctrl+C (copy).
I then exit TCEDIT without saving, and use the CLIP: contents as needed.

While I have not tried this, TCEDIT can also be used to edit files on FTP and HTTP sites.

An alternate method of making changes to the clipboard; 

```dos
type clip: | tcedit
```


Ref: https://jpsoft.com/forums/threads/tcedit-the-clipboard.10520/#post-59269
