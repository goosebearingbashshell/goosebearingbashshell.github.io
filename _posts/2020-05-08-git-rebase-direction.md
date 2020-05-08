---
title: "Note to self: what is the direction of a git rebase?"
layout: post
---

### tl;dr

```bash
git checkout BRANCH-TO-REBASE
git rebase ON-TOP-OF [BRANCH-TO-REBASE]
```

Is what I almost always use. If it was for this usage only, there should just
be a command named `git rebase-on` to clear all confusion.

### Ranting

Sure, I have read the manpage. Now I'm sorry to say, the meaning of the `git
rebase` arguments is quite abstruse:

I removed irrelevant options:

```man
       git rebase [--onto <newbase>]
               [<upstream> [<branch>]]
```

In particular, the existence of the `--onto` option has always confused me,
because I thought that calling `git rebase master` precisely meant "rebase on
master".

Anyway, thank you `git` for providing us with commands so versatile that each
time I need to rebase I spend 15 minutes trying to understand _again_ the
manpage.
