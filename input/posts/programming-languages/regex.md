Title: Regular Expressions
Published: 05/18/2018
Tags:
  - editors
  - programming languages
---
## Notepad++
`notepad++` flavor uses `\1`, `\2` and so on to refer to matched terms under `(.*)`.

Example table,

| Intent                               |          Find         |    Replace   |
|--------------------------------------|-----------------------|--------------|
| Replace `NSLog(@x)` with `printf(x)` | `(NSLog\(@)(.*) (;)`  | `printf(\2`  |
| Remove line numbers from output of bash `history` cmd | `^  7..` | `null`   |
| Converting a batch script variables to Powershell variables | `^([0-9][0-9][0-9] )(.*)`  | `\2`  |
| Remove number prefixes from bash history file | `^\s*(\d|\d\d|\d\d\d)\s\s(\S)` | `\2` |
| Copying from github which contains line numbers as prefixes, then, removing line numbers | `^([0-9][0-9][0-9] )(.*)` | `\2` |
| Given dependency repos name list, regular expression for clone repo | `(.*)` | `clone_repo /builds/atiq/jv8/\1 \1 false` |

`null` above represents empty string.

## npp regex refs
- [notepad-how-to-use-regular-expressions](http://markantoniou.blogspot.com/2008/06/notepad-how-to-use-regular-expressions.html)
- [npp official regex instructions](http://docs.notepad-plus-plus.org/index.php/Regular_Expressions)

**Notepad++ Trips and Tricks**
[Flip or reverse line order in notepad++](https://superuser.com/questions/331098/flip-or-reverse-line-order-in-notepad). It is used in [soln](https://github.com/atiq-cs/Problem-Solving/blob/master/general-solving/leetcode/0012_integer-to-roman.cs) of the problem: [12. Integer to Roman](https://leetcode.com/problems/integer-to-roman)
