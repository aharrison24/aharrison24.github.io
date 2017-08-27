---
layout: post
title: Untangling git submodules
tags: [git, submodules]
---
Sometimes when you move git submodule directories around, things can get
in an inconsistent state, and you’ll receive error messages like:

```
fatal: Could not chdir to '../../../my-submodule': No such file or directory
fatal: 'git status --porcelain' failed in submodule my-submodule
```

These are the places you should look for path inconsistencies:

### `.gitmodules`
Note that a submodule can have a name that is different from the ‘Path‘
entry in .gitmodules. That results in it being stored under a different
repo name in .git/modules/.

### `<submodule>/.git`
In submodule projects this is a file that contains the path to a git
repo stored in the superproject’s .git/modules/ directory. Make sure
this points to a valid path. Quickest fix is to delete the whole
submodule directory and run
```bash
git submodule update --init
```

That should update the .git file and check out a fresh working copy.

### `.git/config`
When you initialise a submodule an entry is added for it in the
superproject’s git config file.

### `.git/modules`
This is where the superproject stores the repositories for all of the submodules.

### `.git/modules/<submodule>/config`
Sometimes the worktree paths in the submodule’s config file can end up pointing at the wrong place.
