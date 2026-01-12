---
layout: default
title: "Linux"
parent: Tutorials
sort: 3
---

# Alias
In Linux, an alias is essentially a custom shortcut or a "nickname" for a command. It allows you to replace a long, complex, or hard-to-remember command with a short, simple word of your choosing.

Open ~/.bashrc, scroll to the very bottom of the file and paste this line:

```bash
alias cleanros='rm -rf ~/.ros/log/*'
```

Apply the changes:
```bash
source ~/.bashrc
```

## How it Works: The "Current Shell" Difference
`source ~/.bashrc` tells your current terminal window to "re-read" its configuration file and apply any changes you just made immediately. Without this command, you would have to close your terminal and open a new one to see your new aliases or environment variables.

To understand why we use source, you have to understand how Linux runs scripts:
- **Normal Execution (./script.sh or bash script.sh)**: This creates a subshell (a new, temporary process). Any changes made inside that script—like a new alias or a variable—stay inside that temporary process and vanish the moment the script finishes.

- **Sourcing (source script.sh or . script.sh)**: This executes the commands inside your current shell. It’s like you are manually typing every line of the file directly into your prompt. Because it happens in your current session, the changes "stick.