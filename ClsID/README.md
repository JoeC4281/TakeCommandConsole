# ClsID.BTM
Given the ProgID, display the method and properties for said ProgID

NOTE:
On my system, I have the following function declared in my PowerShell Profile;

```dos
function CreateObject($progID)
 {
 	new-object -com $progID
 }
```

I call the <code>CreateObject</code> function from the <b>ClsID.BTM</b> code.

```dos
@setlocal
@echo off
iff %# eq 0 then
  :: Display a list of COM dlls that start with jlc
  reg query HKEY_CLASSES_ROOT |! ffind /kvmt"jlc"
  quit
endiff
set ClsID=%@regquery[HKEY_CLASSES_ROOT\%1\clsid\]
iff %ClsID eq -1 then
  echo You need to supply the name of your COM Class.
  echo   For example, jlcUtils.clsMath
  quit
endiff
echo ClsID: %ClsID
iff %@regexist[HKey_Classes_Root\Wow6432Node\AppID\%ClsID\] eq 1 then
  reg add HKey_Classes_Root\Wow6432Node\AppID\%ClsID /v DllSurrogate /f
  reg query HKey_Classes_Root\Wow6432Node\AppID\%ClsID /v DllSurrogate
  pshell /s "$jlc = CreateObject('%1')"
  pshell /s "$jlc | get-member"
else
  echo HKey_Classes_Root\Wow6432Node\AppID\%ClsID\ does not exist.
endiff
endlocal
```
