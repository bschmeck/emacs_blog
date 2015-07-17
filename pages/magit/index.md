---
layout: page
title: Magit
permalink: magit/
---

When I use git, 98% of the time I'm using [Magit](http://www.github.com/magit/magit).

The Magit status buffer shows staged and unstaged changes; think of it as a combination of git diff and git status.  Except, it's better than that, because it allows you to show and hide portions of the diff -- no more hitting `<TAB>` 5 times to complete a file name.  The killer feature of Magit stems directly from that: the ability to interactively stage (or kill) hunks from a diff.  The ease with which I can do this has led to many small commits in my git history, instead of fewer large commits.  Each small commit, then, represents a single logical change, which makes the git history easier to grok and bugs easier to bisect.

Other great features of Magit are:

* Amending a commit is as easy as making a new commit.
* Automatically creating commit messages that work with the autosquash flag for interactive rebasing.
* Jump directly from the git log to a commit's diff.

### Configuration

I use a vanilla installation of Magit, the only Magit related entry in my .emacs binds it to `C-x g`:

~~~ elisp
 ;; ===== Magit =========
 (require 'magit)
 (global-set-key (kbd "C-x g") 'magit-status)
~~~

### Features

Magit has extensive documentation, which I won't try to duplicate here.  I'll just highlight the way I use it, and point out some tricks you might gloss over when reading the docs.

#### Regular Committing

With staged changes, type `c`, then choose the way you want to commit.

* `c` for a normal commit.  This is the common case.
* `a` to amend, adding the staged changes to the current `HEAD`
* `f` to fixup a previous commit.  See Autosquashing for more information.
* `s` to squash a previous commit.  See Autosquashing for more information.

#### Autosquashing Commits

Git's interactive rebasing allows you to modify your commit history, via reordering and combining commits.  Normally, git presents you with an editor window listing the commits to be rebased, you rearrange the file's lines, save the file and git works its magic.  Specially formatted commit messages, combined with the `--autosquash` option, will cause git to do much of the rearranging for you.  And Magit can automatically create those special commit messages for you.  Here's how it works:

After choosing the `f` or `s` option to fixup or squash a commit, Magit pops a buffer showing the commit history for your repo.  Place the point on the line of the commit you want to fixup or squash and select it with `.`.  That's it.  Magit will automatically commit your changes with the special message git rebase is expecting.  All that's left is running the interactive rebase.

(Fixup will automatically use the previous commit's message, squashing allows you to write a new one.)

#### Miscellaneous

`C-w` Copy git SHA at the current point

