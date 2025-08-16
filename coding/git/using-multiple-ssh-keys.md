I use one computer for both my personal and business projects. This means I have multiple
github accounts that I use on the computer. The problem with that is that I need multiple
SSH keys each connecting to a different github account.

I needed a nice solution on how to use multiple ssh keys for github.

First create your 2 ssh keys

```bash
~/.ssh/id_rsa_personal
~/.ssh/id_rsa_work
```

## Git Config
We can use the power of the git config file `~/.gitconfig` to set the ssh key to use for a certain account.

The easiest way I've found we do this is by having different folders for your personal and work projects.

```bash
~/code/personal
~/code/work
```

This way we can change the SSH key that we use depending on the folder the git repo is in and we can do all this from within the Git Config.

Here is an example of my git config file.

```bash
[user]
  name = Paul
  email = test@email.com

[github]
  user = paulund

[core]
  sshCommand = ssh -i ~/.ssh/id_rsa
  excludesfile = ~/.gitignore_global
  editor = vim
  filemode = false
  trustctime = false
    autocrlf = input

[init]
    defaultBranch = main

[includeIf "gitdir:~/Work/"]
  path = ~/work.gitconfig
```

Notice the `[includeIf "gitdir:~/Work/"]` section of the config file. This is telling git to include the config file `work.gitconfig` when the git directory is in the `~/Work/` folder. Now I can put all my work specific settings in this work and make sure all my work projects are inside this work folder.

The work.gitconfig file looks like this.

```bash
[user]
    email = test@email.com
[core]
    sshCommand = "ssh -i ~/.ssh/github-work"
[github]
    user = paulund
```

These settings will override the settings in the main git config file when the git directory is in the `~/Work/` folder.

## How To Create Multiple SSH Keys
To create multiple ssh keys you need to create a new ssh key and add it to your ssh agent.

```bash
ssh-keygen -t rsa -b 4096 -C "
```

When you run this command you will be asked to enter a file name for the ssh key. You can enter a different file name for the ssh key. This will create a new ssh key that you can use for a different github account.

When you have created the ssh key you need to add it to your ssh agent.

```bash
ssh-add ~/.ssh/github-work
```

This will add the ssh key to your ssh agent and you can now use this ssh key to connect to github.

## Using SSH Key For Github
To use the ssh key for github you need to add the ssh key to your github account.

Go to your github account and click on the settings tab. Then click on the SSH and GPG keys tab. Click on the new SSH key button and add the public key of the ssh key you created.

```bash
cat ~/.ssh/github-work.pub
```

This will show you the public key of the ssh key you created. Copy this key and add it to your github account.

Now when you push to github it will use the ssh key you added to your ssh agent.

## Switching Between Accounts
If you need to manually switch between accounts you can use the `ssh-add` command to add the ssh key to your ssh agent.

```bash
ssh-add ~/.ssh/github-personal
ssh-add ~/.ssh/github-work
```

This will add the ssh key to your ssh agent and you can now use this ssh key to connect to github.

## Security Concerns
When you use multiple ssh keys you need to be careful that you don't accidentally push to the wrong account. Make sure you are using the correct ssh key for the correct account.

To make sure that you are using the correct ssh key you can use the `ssh-add -l` command to list the ssh keys that are added to your ssh agent.

```bash
ssh-add -l
```

This will list the ssh keys that are added to your ssh agent. Make sure that you are using the correct ssh key for the correct account.

You can also test a connection to github to make sure you are using the correct ssh key.

```bash
ssh -T git@github.com
```

This will test the connection to github and make sure you are using the correct ssh key.

## Conclusion
This is a great way to manage multiple ssh keys for github accounts. It's a simple solution that allows you to have different ssh keys for different github accounts. This way you can keep your personal and work projects separate and not have to worry about pushing to the wrong account.
