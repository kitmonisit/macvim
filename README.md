# macvim-breakindent

This fork of MacVim has the `breakindent` patch applied to it.

## Versioning

The tagged commit is already the patched version.  All that is left to
do is to compile.  The tag convention is 

    snapshot-<x>_breakindent-<y>

where `x` is the snapshot number from b4winckler/macvim  
      `y` is the `breakindent` patch version from [retracile.net |
      vim](https://retracile.net/blog/category/vim)

## Compiling

My personal compile procedure is

    :::bash
    $ git clone https://github.com/kitmonisit/macvim-breakindent.git macvim
    $ git tag      # see the latest tag with the breakindent patch
    $ git checkout snapshot-72_breakindent-7.4.16-fc19
    $ cd macvim/src
    $ ./configure --enable-pythoninterp \
                  --with-python-config-dir=$(brew --prefix)/Cellar/python/2.7.6/Frameworks/Python.framework/Versions/Current/lib/python2.7/config \
                  --enable-rubyinterp \
                  --enable-perlinterp \
                  --enable-cscope \
                  --with-features=huge
    $ make

## Installing

The `MacVim.app` is found in

    src/MacVim/build/Release

This app bundle can be copied anywhere.

## Usage

- If you want to use the GUI, double-click `MacVim.app`
- If you want to use the console version, the full path is

    MacVim.app/Contents/MacOS/Vim

# Known issues

## Bad Command-T

### Symptom/s

- MacVim GUI will start up, but does not show the text editor itself
- Vim console version crashes and spits out the following error:

        Vim: Caught deadly signal SEGV
        Vim: Finished.
        Segmentation fault: 11

### Cause

Your `command-t` ruby plugin has an architecture mismatch. 

### Solution

This is taken from [Dean Gerber | Vim: Caught Deadly Signal SEGV](http://deangerber.com/blog/2012/01/09/vim-caught-deadly-signal-segv/).  You will need to recompile your `command-t` plugin.  Assuming your `.vim` folder is in your `$HOME` folder:

    $ cd ~/.vim/ruby/command-t
    $ make clean
    $ env ARCHFLAGS="-arch x86_64" ruby extconf.rb
    $ env ARCHFLAGS="-arch x86_64" make
