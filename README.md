## Clone the dotfiles 

> If you are on Mojave the shell will be bash, woohoo! The defualt interactive shell in Catalina (10.15) is zsh and is covered more in [wikipedia](https://en.wikipedia.org/wiki/Z_shell)
This repository is a more thorough examination of ~/.dotfiles to be more specific:
- [Here](https://www.addictivetips.com/mac-os/hide-default-interactive-shell-is-now-zsh-in-terminal-on-macos/) [are](http://zsh.sourceforge.net/) [a](https://discussions.apple.com/thread/250911992) [few](https://www.howtogeek.com/362409/what-is-zsh-and-why-should-you-use-it-instead-of-bash/) [helpful](http://zsh.sourceforge.net/Doc/Release/index.html#:~:text=Zsh%20is%20able%20to%20emulate,last%20updated%20February%2014%2C%202020.) [links](https://github.com/koalaman/shellcheck/)

Type ```git``` and you should see something like this -> follow the steps that are listed in the windows to install the *command line tools*.

![Imgur](https://i.imgur.com/hgheXKb.png)
![Imgur](https://i.imgur.com/hkn1u9d.png)

#### With Git
```bash 
git clone https://github.com/eBenNewell/configuration.git && cd configuration
```

run source the ```bootsrap.sh``` file to strap your system. 

```bash
source bootsrap.sh
```
Next:

- ```cd``` to send you up to your ~/HOME
- ``` chmod +x installer && ./installer.sh``` 
- you'll want to ```exit``` or ```logout``` and press the red x in the top left corner. 



#### With Curl
```
cd; curl -#L https://github.com/eBenNewell/configuration/tarball/master | tar -xzv --strip-components 1 --exclude={README.md,bootstrap.sh,.osx,LICENSE.txt}
```


### Update
```bash
source bootstrap.sh
```
### Avoid the confirmation prompt. 
```bash
set -- -f; source bootstrap.sh
```

####
[(Or, for forking and synchronizing this repository e.g. keeping it up to date)](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/syncing-a-fork)

# Dotfiles Checklist : *Personal Settings*

## Steps / To-Do

1. Launch zsh. ```zsh```

2. chsh -s $(which zsh) "The default interactive shell is now zsh (as of 10.14.x) To update your account to use zsh, please run `chsh -s /bin/zsh`. For more details, please visit [Use zsh as the default shell on your Mac](https://support.apple.com/kb/HT208050). Like to use `echo $SHELL` when it is not clear what shell is being used. 

3. Keep your files in a seperate directory and copy or link them into place. Like [mdo/config](https://github.com/mdo/config/blob/master/config.sh) the way Mark sets up his home directory on any system that he uses with a $HOME/github_dir and a $HOME/work_dir. The github_dir could be used storing dotfiles since the VCS used here is git. We will sym link or copy the files to our 🏠 folder.  

4. These tools are additional dependencies that need to be installed prior to setting up your dotfiles. We do this first so that we have the latest versions of all the tools available. However tmux needs an older version which we will finangle with a script and a little git or homebrew, whichever you prefer to use. After we install the latest version, install XCode from the App store and run `xcode-select --install` to install command line tools. This will take a while with a small circuit at your house (connection), make sure to agree to the license so we can proceed to the next step. 

5. Keep in mind what comes next can be done in any order.

# Steps

## 1. bootstrap.sh (bootstrap installs things.) 

This step is important: bootstrap loader

Jump into the git directory and start rsync’ing the files to your home directory. Then, source on the zshenv file. 

Error handling and garbage collection. 

If forced do not call prompt. 

Becuase this script gets called in the same command that clones the repository to the system you will need this to "hop" into the directory "after" cloning it. Curl: the "cd;" commanad by itself will always put you in your home directory and the bootstrap script will handle copying the files into the home directory for you because you are using the git command. [cd (command)](https://en.wikipedia.org/wiki/Cd_(command)).

Manual inspection: bootstrap loader 

`cd "$( dirname "${BASH_SOURCE[0]}" )" >/dev/null && pwd;`
This is command #0 when thinking of the script in the terms of the source command which runs everything here as it passes them as its arguements. 

`git pull origin master;`
This is arguement #1 and does not use the origin/main convention from git parlance to pull from a remote branch in case there is a cached local version that you want to avoid.

`function sync()`
This function sync() is great, allowing for error checking later (see "if" statement below) and rsync's the repository to your home ~ and sources from . (source) i.e. current directory. Source and destination ( . source ) ( ~ destination ) 

`if [ -n "$ZSH_VERSION" ]; then`
Error handling. Checking the shell.

`if [ "$1" == "--force" -o "$1" == "-f" ]; then`
More error handling - either way if you don't agree to the --force the git pull command which I am still not sure about the sync() function will be executed if you agree to the prompt.

`unset sync;`
The "sync" function variable is not kept. Use unset for garbage collection. 

## 2. ./brew.sh

## 3. ./.macos

> Pass your values through ```defaults``` to set-up system preferences. 

## 4. ./tmux.sh

Enter password when prompted. When finished: `tmux` and `exit` to ensure it is working.

With [.tmux](https://github.com/gpakosz/.tmux#:~:text=tmux%20%3E=%202.1%20(soon%20%3E=%202.4)%20running%20inside%20Linux,%20Mac,%20OpenBSD,%20Cygwin%20or%20WSL) you will need an older version and this can be tricky but the scirpt should take care of that. 

## 5. .tmux/ 

[.tmux](https://github.com/gpakosz/.tmux) - a few adjustments can be made after installation. You can also adjust the instrcutions listed below to install the git repository in your dotfiles folder and link or copy the conf files from there instead of directly in your home directory. If your home directory is a git repository anyway, I am sure you know that anything not listed to igrnore during a `git clean` will be removed. Then `tmux` and `exit` to ensure it is working.

```bash
$ cd
$ git clone https://github.com/gpakosz/.tmux.git
$ ln -s -f .tmux/.tmux.conf
$ cp .tmux/.tmux.conf.local .
```

## 6. [oh-my-zsh](https://ohmyz.sh/)

[Prerequisites](https://github.com/ohmyzsh/ohmyzsh#:~:text=Prerequisites%0A%0A%20%20%20%20A%20Unix-like%20operating%20system:%20macOS,%20Linux,%20BSD.%20On%20Windows:%20WSL2%20is%20preferred,%20but%20cygwin%20or%20msys%20also%20mostly%20work.%0A%20%20%20%20Zsh%20should%20be%20installed%20(v4.3.9%20or%20more%20recent%20is%20fine%20but%20we%20prefer%205.0.8%20and%20newer).%20If%20not%20pre-installed%20(run%20zsh%20--version%20to%20confirm),%20check%20the%20following%20wiki%20instructions%20here:%20Installing%20ZSH%0A%20%20%20%20curl%20or%20wget%20should%20be%20installed%0A%20%20%20%20git%20should%20be%20installed%20(recommended%20v2.4.11%20or%20higher))

Follow the instructions and use one of the three ways that are listed in the instructions to install oh-my-zsh. 

#### via curl

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### via wget

```shell
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### via fetch

```shell
sh -c "$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## 7. [gpakosz/.vim](https://github.com/gpakosz/.vim)
#### If you want to install the configuration

```bash
$ cd
$ rm -rf .vim
$ git clone https://github.com/gpakosz/.vim.git
$ ln -s .vim/.vimrc
```

#### Heavenly branch (*grin*)

```bash
$ cd .vim
$ git checkout heavenly
$ git submodule init && git submodule update
```

### dot files & rc (resource files for runtime)
| Locaiton                   | Descritpion                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ---                        | ---                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `~./dotfiles`                | Installation With your dotfiles in their own repository, there are two possible ways to install dotfiles on systems: copying or symbolically linking files. Symbolic links are better — using symlinks, there is no need to manage discrepancies between copies. Changes to configuration files are changes to the working copy in the repository.
| `brew`                     | This is taken care of inside of the installation script (so far) and looks like : `mkdir homebrew && curl -L https://github.com/Homebrew/brew/tarball/master                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | tar xz --strip 1 -C homebrew` always. Thanks to Marks error handling. 
| `.gitconfig`               | [Creating your global gitconfig file](https://digitalfortress.tech/tutorial/create-global-gitconfig-git-alias/) & [Global gitconfig base file](https://github.com/niketpathak/devpreferences/blob/master/git/.gitconfig)
| `.bash_profile or .zshenv` | [Migrate the right way](https://carlosroso.com/the-right-way-to-migrate-your-bash-profile-to-zsh/) if you are going to do a wipe and fresh install say from Mojave to Catalina, 10.14.x to 10.15.x.
| `.zshrc`                   |
| `.zshenv`                  | First file to be evaluated
| `oh my zsh`                | [Repository](https://github.com/ohmyzsh/ohmyzsh) and help with installing @ [medium post](https://medium.com/@shivam1/make-your-terminal-beautiful-and-fast-with-zsh-shell-and-powerlevel10k-6484461c6efb)
| `.vim`                     | Custom .vim thanks to [gpakosz](https://github.com/gpakosz/.vim.git) Links a .vimrc to your home ~/$(HOME), so clone anywhere, I would mkdir a ~/project folder or something similar to that. Use the git submodule init && git submodule update. From: https://www.vogella.com/tutorials/GitSubmodules/article.html Use the git submodule update command to set the submodules to the commit specified by the main repository. This means that if you pull in new changes in to the submodules, you need to create a new commit in your main repository in order to track the updates of the nested submodules." Exerpt from git documentation: "Submodules allow you to keep a Git repository as a subdirectory of another Git repository. This lets you clone another repository into your project and keep your commits seperate. Link: https://git-scm.com/book/en/v2/Git-Tools-Submodules
| `install tmux 2.x`         | eBenNewell/configuration/tmux.sh thanks to someone, I can't remember but I found this in an old tmux issue thread on github and it has been so long, can't find it anymore. I have it kept seperate from the inital install_script(s).sh for now. Until I find the source or just give up and merge it with eBenNewell/config/install_script.sh and leave a little note at the top.
| `.tmux_conf`               | [Install](https://github.com/gpakosz/.tmux) and do something like mkdir ~/tmuxbuild && cd ~/tmuxbuil. If you have made a mistake and already have tmux > 3.x then try to install with a little help and https://medium.com/@wpcarro/brewing-an-old-batch-of-tmux-81c0a62715f9 `brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/207dad3f513901d6457ffd3c80bf075006789359/Formula/tmux.rb` some code in your shell like this to see if the hash for 2.1 will work for downloaded tmux@2.1 or https://stackoverflow.com/questions/3987683/homebrew-install-specific-version-of-formula look here for notes on simple switching in homebrew. This article has proven to be invaluable. And if this creates more problems, try: https://gist.github.com/shrayasr/8714601
| `.zlogin`                  | 
| `.zlogout`                 | 
| `.zprofile`                |
# Troubleshooting

It's likely you are not able to find a certain command! Here are a few things you can check... modify the ```PATH``` variable in ~/.zshenv then open a new window or tab. Conflict between files are a common problem when cloning a repository full of dotfiles (not to mention switching to zsh from bash and using ./oh-my-zsh... oh my!), for example shopt no longer exists in zsh, with the enrichment that it is agreed that zsh brings to the *nix environemnt, one might not think that to be the case. When you are defining the aliases in your ```$HOME.zshrc```, first, check whether your aliases are defined after your sourcing oh-my-zsh.sh. If you are using a custom file like ```.aliases```, make sure that it's set as the ```$ZSH_CUSTOM``` variable in ```$HOME/.zshrc```. Or source the file after the source oh-my-zsh.sh line.

You can remain on the ancient Bash 3.5.2 that dates back to 2007 -- if you wish, as Apple is not removing it from Catalina.

> An *interactive shell*

__Start Up__ files are highly tailorable in bash/zsh.

For exmaple:

Cron shell scripts are always non-login and non-interactive.
SSH shells are always login and interactive.
Subshells are always interactive but not login.
Shell scripts are always non-login and non-interactive .
Initial TTY shell should be login and interactive.

# Bash
Since ```.bashrc``` is read on interactive but not login, and ```.bash_profile``` is read on all login shelss.
```
#!/usr/bin/env zsh
if [[ -o login ]]; then 
        print yes
    else
        print no
    fi
```

As /etc/zshenv is run for all instances of zsh, it is important that it be kept as small as possible. In particular, it is a good idea to put code that does not need to be run for every single shell begind a test of the form 'if [[ -o rcs ]]; then ...' so that it will not be executed when zsh is invoked with the '-f' option.


## Many Thanks And Appreciation 
* [Mark Otto](https://github.com/mdo/config)
* [Mathias’s dotfiles](https://github.com/mathiasbynens/dotfiles)
* [George Hotz](https://github.com/geohot/configuration)
* [MikeMcQuaid/strap](https://github.com/MikeMcQuaid/strap)
* [cowboy/dotfiles](https://github.com/cowboy/dotfiles)
* [gf3/dotfiles](https://github.com/gf3/dotfiles)
* [matijs/dotfiles](https://github.com/matijs/dotfiles)
* [Kevin Suttles awesome defaults project](https://github.com/kevinSuttle/macOS-Defaults)
* [Gregory Pakosz @gpakosz](https://github.com/gpakosz)
- vim @ [gpakosz/.vim](https://github.com/gpakosz/.vim)
- tmux @ [gpakosz/.tmux](https://github.com/gpakosz/.tmux)
* [oh my zsh](https://github.com/ohmyzsh/ohmyzsh)
* [niketpathak/devpreferences](https://github.com/niketpathak/devpreferences/blob/master/git/.gitconfig)
* [nikitavoloboev](https://github.com/nikitavoloboev/dotfiles)
## Resources

* [The Unofficial Guide](https://dotfiles.github.io/bootstrap/)
* [Startup/Shutdown](http://zsh.sourceforge.net/Doc/Release/Files.html)
* [The config files of random github users, *not* specific at all] (https://github.com/search?q=zsh+dotfiles&ref=commandbar)
## Clone the dotfiles 