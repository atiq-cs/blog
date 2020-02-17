Title: Regular Expressions
Published: 05/18/2018
Tags:
  - editors
  - programming languages
---
## Notepad++
`notepad++` flavor uses `\1`, `\2` and so on to refer to matched terms under `()`.

How to replace NSLog(@x) with printf(x)
In find text box we put,

    (NSLog\(@)(.*) (;)

In replace text box we put,

    printf(\2


Copying from github then removing line numbers in front of all lines
removing them using notepad++ regular expression,

    find        : ^([0-9][0-9][0-9] )(.*)
    replacement : \2

regular expression for clone repo, dependency repos given list of repo names,

    clone_repo /builds/atiq/jv8/\1 \1 false

Remove beginning line numbers from the output of history command,
^  7..  


Remove all numbers from bash history files,

    find        : ^\s*(\d|\d\d|\d\d\d)\s\s(\S)
    replacement : \2

Exammple table,

| Tables   |      Are      |  Cool |
|----------|:-------------:|------:|
| col 1 is |  left-aligned | $1600 |
| col 2 is |    centered   |   $12 |
| col 3 is | right-aligned |    $1 |

## npp regex refs
- [notepad-how-to-use-regular-expressions](http://markantoniou.blogspot.com/2008/06/notepad-how-to-use-regular-expressions.html)
- [npp official regex instructions](http://docs.notepad-plus-plus.org/index.php/Regular_Expressions)

**Notepad++ Trips and Tricks**
[Flip or reverse line order in notepad++](https://superuser.com/questions/331098/flip-or-reverse-line-order-in-notepad). It is used in [soln](https://github.com/atiq-cs/Problem-Solving/blob/master/general-solving/leetcode/0012_integer-to-roman.cs) of the problem: [12. Integer to Roman](https://leetcode.com/problems/integer-to-roman)

