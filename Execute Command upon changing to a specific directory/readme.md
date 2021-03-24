Execute Command upon changing to a specific directory
=====================================================

When I change to my E:\Utils directory, I usually do a directory listing of files modified in the last day.  
  
I have automated this with the following alias;

Code:

    alias cd_enter=`if %1 eq e:\utils dir e:\utils /[d-0]`

  
Reference to CD\_Enter: [ALIAS command - create new command names](https://jpsoft.com/help/alias.htm#cd)  
  
Posting mainly for my future reference, but thought others might also be interested in this tip.  
  
Joe

Ref: https://jpsoft.com/forums/threads/execute-command-upon-changing-to-a-specific-directory.10385/
