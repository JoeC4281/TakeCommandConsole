# AllInOne.btm demonstrates having JScript, VBScript, TCC code in one .BTM file

Windows Scripting Host seems to only care about the package section of a file,<br>
and ignores everything else.

This .BTM can also be used as an .INI file.

Thus, the first part of this .BTM is TCC code,<br>
the second part is VBScript/JScript code,<br>
and the third part is an .INI file.

I'm posting this mainly for my future reference,
but others may be interested also.

I tried to post the code for the .BTM in the forum,<br>
but it would not allow me to post it, saying that I had been blocked.

As the .BTM also contains WSF code (XML code),<br>
I'm thinking that is why I was not allowed to post it.

```dos
@setlocal
@echo off
:: AllInOne.btm
:: Demonstrates having JScript, VBScript, TCC code in one .BTM file
::
:: Windows Scripting Host seems to only care about the package section of a file,
::   and ignores everything else.
::
:: This .BTM can also be used as an .INI file.
::
:: Thus, the first part of this .BTM is TCC code, the second part is
::   VBScript/JScript code, and the third part is an .INI file.
::
iff %_batchtype ne 0 then
  echo This .BTM cannot be compressed or encrypted.
  quit
endiff

iff isalias[cscript64] then
  :: NEXT SENTENCE
else
  alias cscript64=`C:\Windows\system32\cscript.exe //nologo %$`
endiff

COMMENT
iff isalias[cscript32] then
  :: NEXT SENTENCE
else
  alias cscript32=`%_wow64dir\cscript.exe //nologo %$`
endiff
ENDCOMMENT

::
:: The following demonstrates how to request data from a dbase file via SQL.
::   I have REMmed it out, as the DSN and .dbf are specific to my system.
::   
echo ------------
echo Calling Job3 (JScript)
cscript64 //nologo //job:JOB3 "%~F0?.wsf"

COMMENT
:: Demonstration of how to use this .BTM as an .INI file
::   This will write the .INI file data to the end of this .BTM
::
echo %@iniwrite[%_batchname,Main,Amount,10.23] > nul
ENDCOMMENT

:: Call VFP, which is Visual FoxPro
::
echo -----------
echo Calling VFP (VBScript)
timer on
cscript64 //nologo //job:VFP "%~f0?.wsf" %$
timer off
::
:: Call JOB2, which is JScript code
::
echo ------------
echo Calling Job2 (JScript)
iff %# eq 0 then
  cscript64 //nologo //job:JOB2 "%~f0?.wsf" One Two
else
  cscript64 //nologo //job:JOB2 "%~f0?.wsf" %$
endiff
::
:: Call JOB1, which is VBScript code.
::   The arguments are obtained by using this file as an .INI file.
::
echo ------------
echo Calling Job1 (VBScript)
cscript64 //nologo //job:JOB1 "%~f0?.wsf" %@iniread[%_batchname,Main,Amount]
::
:: Call JOB1, which is VBScript code.
::
echo ------------
echo Calling Job1 (VBScript)
cscript64 //nologo //job:JOB1 "%~f0?.wsf" %1
::
:: Call JOB2, which is JScript code
::
echo ------------
echo Calling Job2
cscript64 //nologo //job:JOB2 "%~f0?.wsf" %1
::
:: Call JOB1, and save the returned value as an environment variable.
:: 
echo ------------
echo Store amount to environment variable
set amount=%@execstr[2,cscript64 //nologo //job:JOB1 "%~f0?.wsf" %@iniread[%_batchname,Main,Amount]]
echo The Amount is: %@word[-0,%amount]
::
:: Display the INI file section of this .BTM
::
echo ------------
echo Display the INI file section of this .BTM
tail /n2 %_batchname
endlocal
quit

<package>
  <job id="JOB1">
    <runtime>
	<description>
        This script was run from AllInOne.btm
    </description>
    <unnamed
      name="10.23"
      helpstring="The amount to calculate"
      many="false"
      required="true"
    />
	<example>Example: AllInOne.btm 10.23</example>
    </runtime>
    <script language="VBScript">
      set objArgs = WScript.Arguments
      WScript.Echo "VBScript"
      WScript.StdOut.Write "Argument Count: "
      WScript.Echo objArgs.Length
      if objArgs.Length = 0 then
	    WScript.Arguments.ShowUsage()
	  end if
      if objArgs.Length = 1 then
	    if vb_IsNumeric(CStr(objArgs(0))) then
          WScript.StdOut.Write CStr(objArgs(0)) + " x 1.13 = "
          WScript.Echo objArgs(0)*1.13
		end if
      end if
	  '
	  ' This is a demonstration of calling a UDF
	  '   Just a wrapper for the IsNumeric function.
	  '
	  function vb_IsNumeric(theItem)
	    if IsNumeric(theItem) then
		  vb_IsNumeric = True
		else
          vb_IsNumeric = False
		end if
	  end function
    </script>
  </job>

  <job id="JOB2">
    <script language="JScript">
      objArgs = WScript.Arguments;
      WScript.Echo("JScript");
      WScript.StdOut.Write("Argument Count: ");
      WScript.Echo(objArgs.length);
      if (objArgs.Length == 1)
	    {
          WScript.Echo(objArgs(0));
		}
      if (objArgs.Length == 2)
	    {
          WScript.Echo(objArgs(0));
          WScript.Echo(objArgs(1));
		}
    </script>
  </job>
  
  <job id="JOB3">
    <script language="JScript">
      WScript.Echo("JScript & ADODB from DSN");
      oTFSA = new ActiveXObject("ADODB.Connection");
      oTFSA.Open( "DSN=TFSA" );
      var rc
      rc = oTFSA.Execute( "select sum(interest) as MotusInterest from tfsa.csv where ACCOUNT = 'Motus'" );
      WScript.Echo(rc.Fields("MotusInterest"));
    </script>
  </job>

  <job id="VFP">
    <script language="VBScript">
	  WScript.Echo "VBScript & Visual FoxPro 9"
      Set oVFP = CreateObject("VisualFoxPro.Application.9")
      oVFP.DoCmd("use e:\utils\lottario.dbf")
      MyArray = oVFP.RequestData("lottario")
      WScript.Echo LBound(MyArray,1)
      WScript.Echo LBound(MyArray,2)
      mRecords=UBound(MyArray,1)
      mFields=UBound(MyArray,2)
      oVFP.DoCmd("RecCount=RECCOUNT()")
      RecCount = oVFP.Eval("RecCount")
      oVFP.Quit
      Set oVFP = Nothing
      WScript.StdOut.Write "Records: "
      WScript.StdOut.WriteLine mRecords
      WScript.StdOut.Write " Fields: "
      WScript.StdOut.WriteLine mFields
      For theRecordNumber = 1 to mRecords
        For theFieldNumber = 1 To mFields
          WScript.StdOut.Write MyArray(theRecordNumber,theFieldNumber)
          WScript.StdOut.Write " "
        Next
        WScript.StdOut.WriteLine ""
      Next
      WScript.Quit
	</script>
  </job>

</package>
::
:: This is the .INI file section of this .BTM
::
[Main]
Amount=10.23
```

Ref: https://jpsoft.com/forums/threads/jscript-vbscript-tcc-code-in-one-btm-file.10369/#post-58427
