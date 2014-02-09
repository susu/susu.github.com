{
  title: "Clang completion in vim with YouCompleteMe",
  date: "2014-02-09",
  description: "How to set up VIM/clang completion"
}

*YouCompleteMe* is a plugin for vim. It requires **vim** with **version 7.3.584 at least** and **with python2 support**.
You can check it:
```bash
$ vim --version
```

You should get a similar output. Look for the version number, and for a ```+python```:
```
VIM - Vi IMproved 7.4 (2013 Aug 10, compiled Jan  7 2014 22:31:10)
Included patches: 1-135
Compiled by Arch Linux
Huge version with GTK2 GUI.  Features included (+) or not (-):
... truncated ...
+conceal         +libcall         +profile         +visualextra
+cryptv          +linebreak       +python          +viminfo
+cscope          +lispindent      -python3         +vreplace
+cursorbind      +listcmds        +quickfix        +wildignore
+cursorshape     +localmap        +reltime         +wildmenu
... truncated ...
```

- - -
If you don't have such vim, you can re-compile it (```--with-python2```) or check if your distribution provides other
vim packages. E.g. on Debian you can check ```vim-nox```. On ArchLinux you can recompile via ABS (I had to).
- - -

## Installing with vundle ##

I highly recommend to use [Vundle](https://github.com/gmarik/Vundle.vim). It makes plugin-management super-easy with
vim.

Add to your ```.vimrc```:

```
Bundle 'Valloric/YouCompleteMe'
```
And run:
```bash
$ vim +BundleInstall
```

It may warn you, don't be scared:
```
ycm_client_support.[so|pyd|dll] and ycm_core.[so|pyd|dll] not detected;
you need to compile YCM before using it. Read the docs!
```

Let's compile YCM modules! You can compile both YCM and clang, or you can use your system's libclang, and only compile
YCM. I will elaborate only compiling both.

Make sure you have ```cmake```, ```python-dev``` installed.

```bash
cd ~/.vim/bundle/YouCompleteMe
./install.sh --clang-completer
```

- - -
(It may take you awhile...drink a coffee or create more unit tests for your code!)
- - -

## Setting up for a project ##

In order to use YCM in a project, you have to set the compiler flags (especially include paths, but the same warning
flags are useful as well). Since clang's flags are compatible with GCC's, you can use GCC flags.

You have to provide a ```.ycm_extra_conf.py``` into your project's root (if there isn't already). Recommended to check
it into your VCS so others can use it as well.

There is a base one (recommended to use) in ```.vim/bundle/YouCompleteMe/cpp/ycm/.ycm_extra_conf.py```
All you have to do is provide the correct flags in the ```flags``` variable.

## Known Issues ##

At the time of writing there is a known issue about not loading .ycm_extra_conf.py: [Issue](https://github.com/Valloric/YouCompleteMe/issues/615)

The workaround: add to .vimrc:
```vim
let g:ycm_confirm_extra_conf = 0
```

## Syntastic ##

[Syntastic](https://github.com/scrooloose/syntastic) is a real-time syntax checker. YouCompleteMe integrates very
well with Syntastic.

You can install with Vundle:

```
Bundle 'scrooloose/syntastic'
```

Compile flags in ```.ycm_extra_conf.py``` must be set precisely if you want to avoid false positives/negatives.



