---
layout: post
title: Faster checkouts with git-lfs
tags: [git, git-lfs, checkout]
---
With older versions of git-lfs and git, checkouts can be very slow when new lfs assets need to be fetched. That’s because git checkout only runs the smudge filter sequentially. In comparison, git lfs pull is able to download multiple assets in parallel, which can be much faster.

Here’s a fudge to temporarily disable the git lfs smudge filter, so that git checkout doesn’t try to download all of the files sequentially.

``` bash
# Temporarily turn off the smudge filter
git lfs install --force --skip-smudge

# Do the checkout or pull here
git pull origin master

# Grab the new binary assets from lfs (in parallel)
git lfs pull

# Turn the smudge filter back on
git lfs install --force
```
