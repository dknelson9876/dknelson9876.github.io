---
Title: SSH on CADE
Date: 2022-11-04
Tags: terminal, ssh, vscode
Summary: Guide on how to setup integration between SSH and VScode on my university's lab machines
---

## SSH VScode integration

Me and probably others *dislike* terminal based editors. They have their advantages but for people who have used an IDE all their life it is not always ideal. Here's how to run VSCode so you don't have to use those.

This guide is separated into two steps.
1. Make your ssh keys and setup SSH into a CADE machine
- This one above is kind of optional since its not *required* to make it work, it's just wildly inconvenient without it
2. VSCode extension setup and remote dev install


### 1 SSH Keys

##### Setup OpenSSH

1. Open the **Settings** panel, then click **Apps**
2. Under the _Apps and Features_ heading, click **Optional Features**
3. Scroll down the list to see if **OpenSSH Client** is listed
-   If it’s not, click the plus-sign next to **Add a feature**
-   Scroll through the list to find and select **OpenSSH Client**
-   Finally, click **Install**

##### Common Instructions

1. Open up your terminal emulator (Powershell for Windows, Terminal for MacOS and Linux)
2. Run `ssh-keygen`
- If you already have an ssh key you can reuse that. To check if you have one,  see if you have a directory in your home directory called `.ssh` (`~/.ssh`) with a file called `id_xxx.pub` where `xxx` is an encryption method, commonly `rsa` or `ed25519`
- When running this program you probably don't want to give your key a passphrase, since you don't want to have to entire a password every time we use it
- You can usually just press enter until the program completes
3. Open up your newly generated ssh key pub file, which has the format `id_xxx.pub` from the `~/.ssh` folder. (See 2a for more info).
4. Copy the line that appears. You'll need it for later. This is your **public key**
- It should look *something* like this:
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMRSgiBte9Px92dZCj33fiA/HHpVHnD55/+Y0KB0W7fn stefan.b.todorov@gmail.com
```
5. SSH into your CADE machine. This is done normally, but I'll give helpful links and example command
- Example command: `ssh uxxxxxx@lab1-3.eng.utah.edu`
- [Available target machines](https://www.cade.utah.edu/faqs/which-machines-can-i-access-remotely/)
- The number of the lab machine doesn't actually matter as all info is transferred between them all.
6. Add your **public key** to the `~/.ssh/authorized_keys` file
- The directory `~/.ssh` may not exist and you may need to make it with `mkdir`
- The file `authorized_keys` may not exist and you may need to make it
- I would recommend using `nano` to create the file if you don't like lots of terminal text editors. Exit with `CTRL+X` then `Y` then `ENTER`
- Just paste the **public key** as is, if there's one already, add new line
7. Exit everything!


### 2 VSCode setup

1. Open up VSCode
2. Install these extensions
- `Remote - SSH` [link](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
- `Project Manager` [link](https://marketplace.visualstudio.com/items?itemName=alefragnani.project-manager) *this one is optional but highly recommended*
3. Add a remote SSH Host by opening the command palette (`CTRL + P` or `COMMAND + P` then type `>Add new SSH Host...`)
4. Run the command labeled `Remote-SSH: Add new SSH Host...` (it will show up before you finish typing)
5. Type in the `ssh` command above you used to connect to the CADE machine.
6. Add to whatever config file you'd like, the first one is recommended.
7. After you've set that up, use the command palette to run `Remote-SSH: Connect To Host...`
8. Connect to the machine you just added
- If it's your first machine connection it should be the first option
9. Go to `File > Open Folder` and open any folder, I prefer `~` (your home folder)
10. Go to the Project Manager extension section on the left (looks like two folders)
11. Hover over favorites and click the save icon to save the current place as a "Project"

### You're done!

Seriously that's it. You can use the terminal inside VSCode to do whatever you need to like compiling.

If you need to reconnect to CADE, you don't need to do steps 4-11 again, just go to the Project Manager extension section and click on the profile you just made!

Again, if you have issues leave a discussion below and I will do my best to answer / update the guide.


