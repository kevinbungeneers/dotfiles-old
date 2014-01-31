# Kevin's Dotfiles

These are my dotfiles and contain:

* My fork of Prezto, an awesome configuration framework for ZSH
* Solarized theme for Terminal.app
* Some default settings for OS X
* Powerline patched fonts


## Installation

To install, simply clone this repository into your homedir:

	git clone --recursive https://github.com/kevinbungeneers/dotfiles.git ~/.dotfiles

and run:
	
	rake install

## Uninstall

```
cd ~/.dotfiles && rake uninstall
```

This will remove all the files that were copied or symlinked from the dotfiles directory, including the fonts.

## Credits and thanks

I didn't come up with all of this. This repo is pretty much the result of a lot of googling and browsing other people's dotfile repo.

* http://ethanschoonover.com/solarized
* https://github.com/skwp/dotfiles
* https://github.com/sorin-ionescu/dotfiles
* https://github.com/mathiasbynens/dotfiles