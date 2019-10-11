# Configuration files

That's the repository that helps to maintain my linux desktop environment. Most of the instructions are written for my personal workflow.

## Features

- Customize minimalist IDE with essential tools
- Manage multiple terminal sessions
- Configure and automate development workflow

## Prerequisites

This repository consists a configuration for various applications most of which need the packages listed below:

 1. SVN system -`git`
 2. Based text editor - `vim`
 3. Terminal multiplexer - `tmux`
 4. Session manager - `tmuxp`
 5. Python package installer - `pip`

## Installation

Well, we wanna make `$HOME` the git *work-tree* but we don't want the entire user folder to be in a repo?

That's where a git *bare* repository comes to play, the method I came across with recently. That looks more simple and elegant for versioning your configuration that symlink though.

So let's get started!

### Git Bare Repository

We create a dummy folder and initialize a *bare git repository*, essentially a repo with no working directory in there.

We gonna set an alias with a *dummy folder* for git files and *$HOME* for work directory. We don't mess up other repos and git commands in *$HOME* if we run the git commands this way.

So let's get started!

#### First Time Setup

Create a git bare repository.

```bash
mkdir .dotfiles
git init --bare $HOME/.dotfiles
```
Append the alias to `.bashrc`:

```bash
echo 'alias dotfiles="/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME"' >> $HOME/.bashrc
```

Then using this command add a remote and set *status* not to show untracked files:

```bash
dotfiles config --local status.showUntrackedFiles no
dotfiles remote add origin git@github.com:aubique/dotfiles.git
```

#### Putting Vim on its Feet

This repo contains a [Vundle](https://github.com/gmarik/Vundle.vim) submodule repository. That's an extenstion manager that helps to manage environment with plugins properly.

Get into `vim` and run `:PluginInstall` to download the plugins that you'd marked in `.vimrc`.

### Setting Up a New Machine

There are like two ways of fetching your dotfiles on your new machine. The first one is simply clone to your user folder.

However, in your *$HOME* directory git might find existing config files. So you can clone it to a temporary folder:

```bash
git clone --separate-git-dir=$HOME/.dotfiles https://github.com/aubique/dotfiles.git tmpdotfiles
```

Copy by `rsync` your configs to *$HOME*-directory.

Once we're done with synchronizing our config files we delete temporary folder:

```bash
rsync --recursive --verbose --exclude '.git' tmpdotfiles/ $HOME/
rm -r tmpdotfiles
```

## Sources

I've got quite a lot from [the unofficial guide to dotfiles on GitHub](https://dotfiles.github.io/) to make a backup of my system configuration. And the whole idea of putting Vim to a good use as IDE was inspired particularly by RealPython article called [VIM and Python – A Match Made in Heaven](https://realpython.com/vim-and-python-a-match-made-in-heaven/).

I also used these articles to create my own dotfiles configuration:

- Anand Iyer Blog: [A simpler way to manage your dotfiles](https://www.anand-iyer.com/blog/2018/a-simpler-way-to-manage-your-dotfiles.html)
## TODO
- [ ] Add screenshots of Vim-IDE
- [ ] Merge with `manjaro` branch
