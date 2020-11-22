# ClsID.BTM
Given the ProgID, display the method and properties for said ProgID

NOTE:
On my system, I have the following function declared in my PowerShell Profile;

<code>function CreateObject($progID)
 {
 	new-object -com $progID
 }<code>

I call the CreateObject function from the ClsID.BTM code.
