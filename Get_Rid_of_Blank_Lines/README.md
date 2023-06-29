# Get Rid Of Blank Lines

`wmiquery /a . "select name from Win32_service where ProcessId=1032" | tpipe /simple=10`

Or, create an alias;

`RemoveBlankLines=tpipe /simple=10`

then

`wmiquery /a . "select name from Win32_service where ProcessId=1032" | RemoveBlankLines`

Using TPipe is slow. Using FFIND is faster;

`RemoveBlankLines=findstr /r "."`

Ref: [Get rid of WMIQUERY's blank lines](https://jpsoft.com/forums/threads/get-rid-of-wmiquerys-blank-lines.10663/#post-60023)

Via WayBackMachine: [Get rid of WMIQUERY's blank lines](https://web.archive.org/web/20201217142513/https://jpsoft.com/forums/threads/get-rid-of-wmiquerys-blank-lines.10663/#post-60023)