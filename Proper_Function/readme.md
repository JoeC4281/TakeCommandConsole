# @Proper Function

This is a function that will convert a non-proper case string to proper case.

```dos
proper=%@pshell[(Get-Culture).TextInfo.ToTitleCase("%$".ToLower())]
```
Proof;

```dos
echo %@proper[DAILY EPIDEMIOLOGICAL SUMMARY]

Daily Epidemiological Summary
```
