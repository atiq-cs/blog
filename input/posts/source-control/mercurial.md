Title: Mercurial Useful Commands
Published: 11/14/2019
Tags:
  - mercurial
  - source Control
  - version Control
---
List files changed in previous commit,

    hg show D12345678 --stat --template 'List of files: \n{files}\n'
    hg show tip~2

Show commit log by short-id,

    hg show 501c70899

Fancy syntax, demonstrating spaces, blank lines etc

    hg show D123456 --ignore-all-space --ignore-space-change --ignore-blank-lines --ignore-space-at-eol --exclude "extra_configs/*"

`hg status` has an equivalent. . represents current `HEAD` of bookmark/repo,

    hg status --rev .~1:.

using alias `tip` (=HEAD) here's one more example,

hg status --rev tip~1:tip

#### Examining differentials
show current diff (uncommitted),

    hg diff

show difference w.r.t revisions,

    hg diff --rev tip~1::tip

same as above, but with exclude syntax and demonstrate whilte-space and newline changes,

    hg diff --rev tip~1::tip --ignore-all-space --ignore-space-change --ignore-blank-lines --ignore-space-at-eol --exclude "extra_configs/*"

supparts naming to bookmarks,
```
hg diff --rev D12345~1::D12345
```


_Creating traditional diff/patch file_
trying some patch related cmd, but, it's not working,
```
hg diff --rev tip -p > 'Feature engineering for my awesome project.patch'
```

Status to see file list changed,
```
hg status --rev tip~1:tip
```

#### Rebasing
specifying soure of rebase toward a destination,
```
hg rebase --source e630a3b5 --dest -d master
```

revisions can be named using bookmarks,
```
hg rebase --source D12345-MyCoolFeature --dest master
```

To identify the working directory or specified revision,
```
hg identify
64541997a288 tip events_text_location_node
```

#### Revert
Example to revert a file to previous commit,
```
hg revert --rev tip~1 my_file_path
hg revert --rev master file_path
```

get a clean state of file
similar to git reset all,

    hg revert --all

Following seems awesome,

    hg revert --rev .~1 --all
    hg revert --rev .~1 file_path..
    hg revert --rev .~1 file_path

Revert a file to specific revision (short-id in example, would work with any commit id),
```
hg revert --rev d48959b33 users/tests/tests_seti.py
```

in case of a terrible merge conflict on json files, we can undo that,
```
hg revert --rev .~1 materialized_configs/search/*
```

#### Misc
Find short-id of latest commit,

    hg log -l 1 --template="{node|short} {date} {author}\n"

check log,

    hg log -l 10

Find commit logs by user name,

    hg log -u 'Atiq Rahman'

common cmds,

    hg rebase
    hg fold
    hg forget

this will also delete the file,
```
hg remove
```

hg command line to do checkout with first 9 digits/letters as SHA id of the commit,
```
hg checkout 3d5b62937
```

get hg log upto 10 commits,
```
hg log -l 10
```

checkout to a specific revision and also to delete current change,
```
hg checkout d48959b331d747a10603ebdbef608ff2080a25a0 --clean
```

bring current changes,
```
hg pull
```

### Bookmarks
The other name of branches,
to create and checkout,
```
hg checkout --rev master --bookmark pages_text_location_node
```

rename,
```
hg bookmark --rename old_name new_name
```

And we can use `--force` in case of a conflict.

Just create,
```
hg bookmark --rev master trebek_rule_filter_loc_pages
```

mercurial and webrev (diffs)
Some commands
```
hg config --edit
```
During webRTI, we do,
```
hg commit -m '22720551 aalib should switch from slang to ncurses'
```


Searching inside component projects
We automate some stuff for components using `/opt/onbld/bin/pbchk`,

    hg pbchk
    hg: unknown command 'pbchk'
    Mercurial Distributed SCM

    hg config --edit
    the user name first hg edit --

    $ pbchk
    -ksh: pbchk: not found [No such file or directory]

    hg commit -m remove
    hg recommit
    hg push ssh://user@domain.com//gates/incoming

### mercurial on Solaris (2017-02)
some examples,

    hg serve
    hg view
    hg log -G

    hg tip - absolute
    hg parent
    hg summary - working dir and tons of other thing
    hg reconnect

**Rewriting history**

commit id/hashes will change for changing some of the fields
There are phases
 - public commits are immutable though

amend example,

    hg commit --amend -m "git 2.7.5"

