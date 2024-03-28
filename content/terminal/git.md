---
Title: Git Cheatsheet
Date: 2022-11-03
Tags: terminal, git
slug: git-cheatsheet
Summary: Very basic notes on using Git
---

## Cloning into a non-empty directory
For setting up .dotfiles repo in a new wsl instance

[StackOverflow](https://stackoverflow.com/questions/2411031/how-do-i-clone-into-a-non-empty-directory)

```bash
git init
git remote add origin [url]
git fetch
git reset origin/master
```