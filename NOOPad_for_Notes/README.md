# NOOPad for Notes

NOOP is a command available in the 4UTILS plugin.

The purpose of the command is to do nothing.

The 4UTILS plugin can be downloaded from
ftp://vefatica.net/4plugins/X64/4utils64.zip

Along with <a href="https://jpsoft.com/help/cmdhist.htm" target="_blank">HISTORY</a>,
I use the NOOP command as a NOOPad for simple one line notes.

For example;

```dos
v readme.txt
noop USD $59.97 CAN $79.02 2023-06-28
sandbox.wsb
noop item number:404296918353
type clip:
```
Commands that I type are saved in my HISTORY list.

I use a <a href="https://jpsoft.com/help/localandglobal.htm" target="_blank">Global History</a> list in TCC.

To diplay all NOOPs;

```dos
history | ffind /kvme"^noop "
```
returns...

```dos
noop USD $59.97 CAN $79.02 2023-06-28
noop item number:404296918353
```
I can also view my NOOPs by displaying a popup window of the command history by pressing PageUp.

First, type;

```dos
noop
```

at the command line, and then press the PageUp key.

These are notes that I do not desire to keep,
so I don't care if they are retained.

I find this easier to use from the command line than the Windows 10 <a href="https://apps.microsoft.com/store/detail/microsoft-sticky-notes/9NBLGGH4QGHW?hl=en-us&gl=us" target="_blank">Sticky Notes</a> app.

YMMV
