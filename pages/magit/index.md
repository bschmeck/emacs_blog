---
layout: page
title: Magit
permalink: magit/
top_level: true
topic: magit
blurb: Interactive git in Emacs
---

When I use git, 98% of the time I'm using [Magit](http://magit.vc/).

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

#### Staging

In the Magit status buffer, hide and show file diffs with `TAB` and stage changes with `s`.  Only the section of the diff containing the point will be staged; you can change the granularity of diffs using `+` and `-`.

#### Committing

With staged changes, type `c`, then choose the way you want to commit.

* `c` for a normal commit.  This is the common case.
* `a` to amend, adding the staged changes to the current `HEAD`
* `f` to fixup a previous commit.  See Autosquashing for more information.
* `s` to squash a previous commit.  See Autosquashing for more information.

#### Autosquashing Commits

Git's interactive rebasing allows you to modify your commit history, via reordering and combining commits.  Normally, git presents you with an editor window listing the commits to be rebased, you rearrange the file's lines, save the file and git works its magic.  Specially formatted commit messages, combined with the `--autosquash` option, will cause git to do much of the rearranging for you.  And Magit can automatically create those special commit messages for you.  Here's how it works:

After choosing the `f` or `s` option to fixup or squash a commit, Magit pops a buffer showing the commit history for your repo.  Place the point on the line of the commit you want to fixup or squash and select it with `.`.  That's it.  Magit will automatically commit your changes with the special message git rebase is expecting.  All that's left is running the interactive rebase.

(A fixup will automatically use the previous commit's message, a squash allows you to write a new one.)

#### Cherry Picking

Cherry picking commits from branch together takes a single keystroke in Magit.  Position the point on a commit in the git log and press `A` to cherry pick that commit onto the `HEAD` of the current branch.  That's all there is to it.

#### Interactive Rebase

Magit supports interactive rebasing, but it invokes emacsclient, and I have never looked into running Emacs Server.  The 2% of the time I use git from outside of Magit, I'm doing interactive rebases.

#### Conflicts

Resolving conflicts during merging and rebasing in Magit is identical to resolving conflicts outside of Magit.  During a merge, simply resolve the conflict, stage the changes and commit.  During a rebase, Magit allows you to abort, skip and continue by pressing the `A`, `S` and `C` keys, respectively.

#### Pushing

`P` begins the process of pushing.  You can specify options for the push (force, dry run and setting the upstream,) and then type `P` again to actually push.  If you want to push to a remote that's not origin, begin the process with `C-u P`.  If you want to push to a different branch on the remote, begin with `C-u C-u P`.  (You'll need to specify both the remote and the branch name.)

For example, the command to push a feature branch to a test Heroku environment (assuming the remote is named heroku-test) would be:

`C-u C-u P P heroku-test master`

(I use `C-u` because that is my binding for the `universal-argument` function.  It's listed as a global keybinding, I assume it's the default, out of the box keybinding.)

#### Pulling

`F` begins the process of pulling.  Just like with pushing, you can specify options (force and rebase,) then type `F` again to actually pull.  To specify a remote, prefix the command with `C-u`.

#### Stashing

`z` begins the process of stashing all uncommitted changes.  I never do anything complicated with stashing, so I just use `z z`, which prompts for a description for the stashed changes and then stashes them.

There are two ways of popping stashed changes: `A` will pop the stashed changes and `a` will merely apply them.  The difference is that popping removes the stashed changes from the stash list after applying them (unless they fail to apply.)

#### Miscellaneous

`:` Run arbitraty git commands.  Magit will open the minibuffer and run whatever command you type (Magit will prepend `git` for you.)

`C-w` If there's a git SHA at the current point, copy it into the kill ring.

`x` Perform a soft reset, you'll be prompted (in the minibuffer) for the commit to which head will be reset.

`g` Refresh the current buffer.

`k` Remove the item under the point (when applicable.)  Use this to remove stashed changes from the stash list, to delete untracked files, to discard changes (individual hunks or an entire file) and to delete branches.

`C-u k` Delete unmerged branches.  Magit will complain if you use `k` to delete a branch before it has been merged with master.  Use the `C-u` prefix to tell Magit you know what you're doing.

