# Github Tutorial
*with sprinkles of git* 🍩

## Goals

1. Setup a git directory
2. Generate ssh key
3. Add your ssh key to your Github account
4. Create a Github repo
5. Push to Github ⬆
6. Become expert in Git
    - Learn how to casually talk about Github at house parties (or galas)

### Prologue

Check if `git` is installed
```sh
> git --version # checks version of git if installed
> which git # checks the location of executable
```

**note:** If you get the following error on Mac, then it means you need to reinstall Xcode command line tools

```sh
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

Should be resolved by running

```sh
> xcode-select --install
```

*see [this post](https://ma.ttias.be/mac-os-xcrun-error-invalid-active-developer-path-missing-xcrun/) by Mattias Geniar for more details about debugging this issue*

### Setup a git directory
Any directory can be a git repository! Just navigate to your directory and just run `git init` like so...

```sh
> cd /to/your/awesome/directory
> git init # initialize a git repo
> ls -la # to ensure the .git folder is now in your project
```

You can now check the **current** state of the project by running `git status`, which provides you some information on untracked files (unstaged files), files that have been `add`ed to the staging environment, and any changes made to files that have yet to be staged.

```sh
> git status
# output
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file1
        file2
        some_dir/

nothing added to commit but untracked files present (use "git add" to track)
```

You can add a file to your staging area by simply running `git add <file>`.

```sh
> git add <file> # to add a single file to staging area
> git add . # add all untracked files to the staging area
```

Now running `git status` will show those files added to staging area waiting to be `commit`ted.

```sh
> git status
# output
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   file1
        new file:   some_dir/file
```

Now it's time to commit the current state of your project. 

```sh
> git commit -m "write some descriptive message about the work that went into this commit"
```

And now it is time to set up our remote repository on Github!

### Generate ssh key

Before generating a new ssh key, you should first check to see if you already have any ssh keys present. Just check the `.ssh/` directory located in your home directory.


```sh
ls -la ~/.ssh
# list files if exist
id_rsa.pub
*
*
```

and if any public keys exist, you'll probably see someting like 'id_rsa.pub', 'id_ecdsa.pub' or 'id_ed25519.pub'

*check the [github docs](https://docs.github.com/en/github/authenticating-to-github/checking-for-existing-ssh-keys) for more info*

**If a key doesn't exist or you want to create a new one then you just need to generate one**

To generate an ssh key just run the following line ensuring to replace the email with your Github email address

```sh
> ssh-keygen -t ed25519 -C "your_email@example.com"
```

You'll probably be asked where to store this public/private key pair which should default to the `~/.ssh/` folder. Press `<Enter>` if the default location suffices.

Next enter a passphrase. *Note: it won't show up as you are typing*.

**Now you can add your ssh-key to the ssh-agent to prevent you from typing your password every time you connect to a server.**

Here's a quick [article](https://smallstep.com/blog/ssh-agent-explained/) briefly explaining `ssh-agent`.

First, you will start `ssh-agent` in the background.

```sh
> eval "$(ssh-agent -s)"
```

Then check if you have the config file in your `~/.ssh/` folder. If not, go ahead and create one and open it up (or just open it if you already do).

```sh
> ls ~/.ssh/
> touch ~/.ssh/config # if no config file run
> open ~/.ssh/config
```

Add the following block of text to your opened `config` file

```
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Finally, add your key to the `ssh-agent` by running...

```ssh
> ssh-add -K ~/.ssh/id_ed25519
```

*check the [github docs](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent) for more info*

### Add your ssh key to your Github account

Now you can copy your ssh public key to add it to your Github account

```sh
> pbcopy < ~/.ssh/id_ed25519.pub
```

Then navigate to your Github, click your avatar, go to "Settings" and from Settings, go to the sidebar and find "SSH and GPG keys".

Click "New SSH Key", then provide a title and paste your key in the "key" input textbox.

Click "Add SSH Key" type in your Github password and voila!

You can now test the connection from your terminal by attempting to ssh to Github.

```sh
> ssh -T git@github.com
```

And if all goes well then you should see 

```sh
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

*check the [github docs](https://docs.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account) for more info*