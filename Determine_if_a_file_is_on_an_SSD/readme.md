```dos
setlocal

if %# == 0 (echo Usage: DTYPE.BTM drive_letter[...] & quit)

:: DriveLetter is the ASCII code of an uppercase letter; e.g., the c drive is 67

set letter=%@ascii[%@upper[%@instr[0,1,%1]]]

set DiskNumber=%@wmi[root\Microsoft\Windows\Storage,"select DiskNumber from MSFT_Partition where DriveLetter=%letter"]

if "%DiskNumber" == "" ( echoerr No such disk & quit )

set MediaType=%@wmi[root\Microsoft\Windows\Storage,"select MediaType from MSFT_PhysicalDisk where DeviceId=%DiskNumber"]

if "MediaType" == "" (echoerr Unknown error & quit)

switch %MediaType
    case 0
        echo UNSPECIFIED
    case 3
        echo HHD
    case 4
        echo SSD
    case 5
        echo SCM
    default
        echo UNKNOWN
endswitch
```

Ref: [Determine if a file is on an SSD](https://jpsoft.com/forums/threads/determine-if-a-file-is-on-an-ssd-drive.10568/#post-59569)

Via WayBackMachine: [Determine if a file is on an SSD](https://web.archive.org/web/20201217155509/https://jpsoft.com/forums/threads/determine-if-a-file-is-on-an-ssd-drive.10568/#post-59569)