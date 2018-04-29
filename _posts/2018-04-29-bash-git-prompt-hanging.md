---
layout: post
title: Stopping bash-git-prompt from hanging in git dirs with many untracked files
tags: [path, bash-git-prompt, git, yadm]
---
If you navigate to a git repository containing a large number of untracked files,
`bash-git-prompt` can become excessively slow, making the terminal unusable. This is
a particular problem when navigating to a [yadm](https://github.com/TheLocehiliosan/yadm)
repository which is tracking the entire `$HOME` directory.

One solution is to tell `bash-git-prompt` not to enumerate untracked files.
```bash
export GIT_PROMPT_SHOW_UNTRACKED_FILES=no
```

