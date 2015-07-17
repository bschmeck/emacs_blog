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

#### Autosquashing Commits

* `f` to fixup.  You'll be presented with a buffer showing the git log, select which commit you want to fixup using `.`.  You'll still need to run an interactive rebase to actually fixup the selected commit.  Using the `--autosquash` option will

#### Miscellaneous

`C-w` Copy git SHA at the current point

