---
Title: Windows Terminal Cheatsheet
Date: 2022-11-04
Tags: 
- terminal
- winterm
slug: winterm-cheatsheet
Summary: Cheatsheet on using Windows Terminal
---

## Include SSH Profile

Add the following to `settings.json` inside the `list` property of `profiles`:

```json
{
    "name": "SSH Profile",
    "commandline": "ssh user@machine"
}
```

[Microsoft Documentation](https://learn.microsoft.com/en-us/windows/terminal/tutorials/ssh)
