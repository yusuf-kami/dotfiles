# dotfiles
A collection of scripts and code snippets used to set up my development environment and profiles.

## Installation
In order to use `dotfiles` CLI you must first get the source and from its GitHub [repo](https://github.com/yusuf-kami/dotfiles) with the following commands:
```
 git clone https://github.com/yusuf-kami/dotfiles.git ~/dotfiles
 cd ~/dotfiles
 chmod +x ./dotfiles.sh
 source ./dotfiles.sh
```


## Setup
Setting up the dotfiles is as easy as `source ./dotfiles.sh` in any bash environment.


## Reloading
Add the path of the directory for `./dotfiles.sh` file to your `~/.bash_profile` using the environment variables `DOTFILES_DIR` like below:
```
  #~/.bash_profile
  echo "Storing the dotfiles path to ~/.bash_profile"
  export DOTFILES_DIR=[PATH_TO_DOTFILES_DIR]
  alias dotfiles="pushd $(pwd); cd $DOTFILES_DIR; source ./dotfiles.sh; popd;"

  echo "Loading dotfiles"
  dotfiles
```
In order to reload the dotfile you can simply source the file using the path in the environment variable with the command `dotfiles`.
Setting the `DOTFILES_DIR` environment variable also lets you set up an alias for the [Git Profile Manager](https://github.com/yusuf-kami/dotfiles/blob/master/git/profile.sh) by adding the following line to your `~/.bash_profile`
```
  alias gpf="echo 'Running: Git Profile Manager'; $DOTFILES_DIR/git/profile.sh "
```


## Included
The repository includes the following configuration and tools:
### 1. Bash
#### 1.1. Aliases
This **[`/bash/aliases.sh`](https://github.com/yusuf-kami/dotfiles/blob/master/bash/aliases.sh)** bash script contains all the aliases for the most frequently used commands and sets them for the console.
**NOTE**: It migth be helpfulful to determine the *code* or *script* attached to an *alias* or a *command*. In order to determine the scripts attached to a command/alias use the  command `type` on the command/alias in question.

#### 1.2. Profile
The bash profile script at **[`/bash/profile.sh`](https://github.com/yusuf-kami/dotfiles/blob/master/bash/profile.sh)** is also included to setup the profile and display preference for the terminal.

#### 1.3. Theme
A theme for the bash console I really like called [Dracula](https://draculatheme.com/) is also included.
- You can set up the terminal theme by importing the [profile](https://github.com/yusuf-kami/dotfiles/blob/master/bin/Dracula.terminal) in file `./bin/Dracula.terminal` into the **Terminal**.
- For **iTerm2** you will have to import the [color presets](https://github.com/yusuf-kami/dotfiles/blob/master/bin/Dracula.terminal) in `./bin/Dracula.terminal` into the a new profile and apply it to the console.

These profiles can always be adjusted to your preferences once they are downloaded. The current look of the *Dracula* theme on Terminal is as follows:
![Dracula theme in Terminal](https://github.com/yusuf-kami/dotfiles/blob/master/bin/Dracula.png)

#### 1.4 `tmux`
You can also use `tmux` which is also installed within the bash setup script. This is a very useful and versatile tool for multiplexing and the script used to install it is provided [here](https://gist.github.com/simme/1297707). To start a multiplexed session just use the command `tmux`.
Once in a `tmux` window in order to run *special commands* you need to input the *prefix* command which is **Ctrl+b**. With this prefix provided new windows or panes can be created and navigated with the following *key binding*:
- create new window: `c`
- create new pane to left: `%`
- create new pane to bottom: `"`
- kill current pane: `x`
- kill current window: `&`
- navigate to next window: `n`
- navigate to previous window: `l`
- navigate to next pane: `o`
- navigate to previous pane: `;`
- show description: `i`
- show pane information: `q`
- change layout: `[space]`
- cycle panes: `Ctr+o`

To view all created windows externally from the `tmux` session use the command `tmux ls`. In order to kill a window the command `tmux kill-window -t {WINDOW_ID}` is used. In order to kill all windows the command `tmux kill-server` can be used.
**NOTE**: To view all possible *key bindings* use the command `tmux list-keys`.

**NOTE**: In order to include the command to clear the screen using the key **Ctl+k** use the command `tmux bind -n C-k send-keys -R \; send-keys C-l \; clear-history`

### 2. Git
#### 2.1. Git Profile Manager
The Git Profile Manager at **[`/git/profile.sh`](https://github.com/yusuf-kami/dotfiles/blob/master/git/profile.sh)** sets the user name and email used to interface with GitHub or Bitbucket. It also sets the Hostname used to connect to GitHub for profiles that have more than one user accounts for the same provider.
In order to be able to use the Git Profile Manager you must set up the alias in your `~/.bash_profile` with the following additions:
```~/.bash_profile
 alias gpf="echo 'Running: Git Profile Manager'; $DOTFILES_DIR/git/profile.sh "
```
The manager can then be run using the `gpf` command in the console.

#### 2.2. Git Rebaser
Included is also a Git Rebaser at **[`/git/rebaser.sh`](https://github.com/yusuf-kami/dotfiles/blob/master/git/rebaser.sh)** that is able to find all the Git directories in the system and performs a `git pull --rebase` command on all the directories.
In order to enable the Rebaser you can add the following alias in the `~/.bash_profile` file:
```~/.bash_profile
 alias grr="echo 'Running: Git Rebaser'; $DOTFILES_DIR/git/rebaser.sh "
```
The Rebaser can then be run using the `grr` command in the console.

#### 2.3 Git Hooks
There are also git hooks included in the **[`/git/`](https://github.com/yusuf-kami/dotfiles/blob/master/git/)** directory. Included are the scripts for the commit message template with the message validation, and the git push hook.
The hooks can be symlinked using the following commands:
```
 rm .git/hooks/*
 ln -fsv "$(PWD)/git/commit-msg" .git/hooks/commit-msg
 ln -fsv "$(PWD)/git/pre-push" .git/hooks/pre-push
```

#### 2.4 Git History
Sometimes there will be an *urgent* need to rewrite the history that has already been pushed to origin. This can be done with the following steps that was obtained from this **Stack Overflow [issue](https://stackoverflow.com/questions/3042437/change-commit-author-at-one-specific-commit)**:
1. Checkout the commit to be updated:
```
  git checkout OLD_COMMIT_HASH
```
2. Make the changes to the commit using `git commit --amend`. With this command you can change even the *Author* of the commit with the `--author` parameter. After this command you should get a new commit hash (NEW_COMMIT_HASH) identifying the introduced changes into the branch.
3. Checkout to the main branch with `git checkout`
4. Replace the old commit with the amended commit:
```
  git replace OLD_COMMIT_HASH NEW_COMMIT_HASH
```
5. Rewrite the history of all future commits in your trunk:
```
  git filter-branch -- --all
```
6. Delete the old commit with `git replace -d OLD_COMMIT_HASH`

7. Push the changes to origin with `git push --force-with-lease`. If there is any issue with the `push` command then you can force the update with `git push --force` instead.
