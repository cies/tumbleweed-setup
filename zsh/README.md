# Zsh

Install and setup the OS dependecies:

```bash
sudo zypper install -y zsh git fzf ripgrep xclip wl-clipboard
sudo usermod -s /bin/zsh $USER
```

**NOTE**: `wl-clipboard` does for Wayland what `xclip` does for Xorg.

Zinit is used as plugin manager (to manage zsh-specific dependencies):

```bash
bash -c "$(curl --fail --show-error --silent --location https://raw.githubusercontent.com/zdharma-continuum/zinit/HEAD/scripts/install.sh)"
```

Skip setting up the additional annexes with `n`.

Finally add some config to `~/.zshrc` with:

```
cat >> ~/.zshrc << EOF
PURE_PROMPT_SYMBOL="$"

zinit for \
    light-mode  zsh-users/zsh-autosuggestions \
    light-mode  zdharma-continuum/fast-syntax-highlighting \
                zsh-users/zsh-completions \
                zdharma-continuum/history-search-multi-word \
    pick"async.zsh" src"pure.zsh" \
                sindresorhus/pure

zinit wait lucid for \
        OMZ::lib/git.zsh \
        OMZ::plugins/ssh-agent \
  atload"unalias grv" \
        OMZ::plugins/git/git.plugin.zsh \
        wfxr/forgit

HISTFILE=~/.zsh_history
HISTSIZE=100000
SAVEHIST=100000
HISTORY_SUBSTRING_SEARCH_ENSURE_UNIQUE=1                # all search results returned will be unique
setopt incappendhistory                                 # add commmand to history as soon as it's entered
setopt extendedhistory                                  # save command timestamp
setopt sharehistory                                     # share history across terminals
setopt HIST_EXPIRE_DUPS_FIRST
setopt HIST_SAVE_NO_DUPS                                # don't write duplicate entries in the history file
setopt HIST_IGNORE_ALL_DUPS
setopt HIST_FIND_NO_DUPS
setopt HIST_IGNORE_SPACE                                # prefix commands you don't want stored with a space
HISTORY_IGNORE="(builtin *|exit|ls|r|open|pwd|q|x *|s *|cd *)"
setopt correct                                          # spelling correction for commands

# Binary release in archive, from GitHub-releases page.
# After automatic unpacking it provides program "fzf".
#zinit ice from"gh-r" as"program"
#zinit load junegunn/fzf-bin

# Handle completions without loading any plugin, see "clist" command.
# This one is to be ran just once, in interactive session.
# NOTE: Commented out as it resulted in errors
#zinit creinstall %HOME/my_completions

# This makes Makefile target completions bearable
zstyle ':completion:*:make:*:targets' call-command true # outputs all possible results for make targets
zstyle ':completion:*:make:*' tag-order targets
zstyle ':completion:*' group-name ''
zstyle ':completion:*:descriptions' format '%B%d%b'

# For GNU ls (the binaries can be gls, gdircolors, e.g. on OS X when installing the
# coreutils package from Homebrew; you can also use https://github.com/ogham/exa)
zinit ice atclone"dircolors -b LS_COLORS > c.zsh" atpull'%atclone' pick"c.zsh" nocompile'!'
zinit light trapd00r/LS_COLORS

alias m=make

# Sets PATH so it includes user's private bin if it exists
if [ -d "\$HOME/bin" ] ; then
    PATH="\$HOME/bin:\$PATH"
fi

# Sets PATH so it includes another private bin if it exists
if [ -d "\$HOME/.local/bin" ] ; then
    PATH="\$HOME/.local/bin:\$PATH"
fi
EOF
```

Simply run `zsh` again to install and setup all that's required by the just pasted `zshrc`.

Some interesting commands:

* `e`: editor
* `ide`: open in ide
* `vi`: open in terminal editor
* `o`: xdg-open
* `f`: fzf find
* `x`: dtrx
* `m`: make
* git shortcut aliasses (`gp`, `gl`, `gco`, `gm`, etc.)
* simulate pbcopy and pbpaste


### TODO

These (and or Wayland equivalents):

```
alias "c=xclip"
alias "v=xclip -o"
alias "pbcopy=xclip"
alias "pbpaste=xclip -o"
```

Also understand history better, and why it seems to not all come to the same place when performed on different tabs.

How to do quick directory jumping with fuzzy something.
