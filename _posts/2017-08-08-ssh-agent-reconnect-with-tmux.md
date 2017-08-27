---
layout: post
title: Reestablish ssh agent when reconnecting to remote tmux session
tags: [ssh, tmux, ssh-agent, reconnect]
---
Drop this in to .bashrc or similar:

``` bash
# Reconnect tmux session to the SSH agent
alias fixssh='export $(tmux showenv SSH_AUTH_SOCK)'
```

Source: [https://stackoverflow.com/a/40967729/4554650](https://stackoverflow.com/a/40967729/4554650)
