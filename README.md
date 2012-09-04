# VimClojure - Easy Peasy Lemon Squeezy
This is a sample configuration for [VimClojure](http://www.vim.org/scripts/script.php?script_id=2501) to hopefully make the process of getting and using VimClojure more straightforward. It is basically a complete vim configuration with VimClojure and nothing else installed. It should be a good sanity check for setting everything up.

## Install
If you have an existing vim configuration, first move it out of the way. You can bring it back in after you feel like VimClojure's working for you.

    $ cd
    $ mv .vim .vim.bak
    $ mv .vimrc .vimrc.bak
    $ mv .gvimrc .gvimrc.bak

Now clone this repo into ~/.vim and set it up:

    $ cd
    $ git clone https://github.com/daveray/vimclojure-easy.git .vim
    $ ln -s .vim/vimrc.vim .vimrc
    $ make -C .vim/lib/vimclojure-nailgun-client
    Building ng client.  To build a Windows binary, type 'make ng.exe'
    gcc -Wall -pedantic -s -O3  -o ng ngclient/ng.c
    ld: warning: option -s is obsolete and being ignored

*Windows users see Windows Notes section below*

## First Test - Syntax Highlight and Stuff

Now open a Clojure file just to see if the plugin's basically working. Conveniently, there's one in this repo:

    $ cd
    $ vim .vim/piglatin.clj

You should see the syntax highlighted source code as well as an error window telling you that VimClojure couldn't find the nailgun server. That's our next step.

If syntax highlighting doesn't work, make sure `.vim` is in your home directory and `~/.vimrc` is a symlink to `~/.vim/vimrc.vim`.

## Starting the Nailgun Server

Now we want to get to the serious stuff. We'll need to start a nailgun server to get the most use out of VimClojure. There are (at least) two options.

The best option is Daniel Solano Gómez's [lein-tarsier plugin] (https://github.com/sattvik/lein-tarsier). First you'll need to install the plugin:

For Leiningen 1.x:

    $ lein plugin install lein-tarsier 0.9.1

For Leiningen 2.x add this to the plugins vector of your `:user` profile located in `~/.lein/profiles.clj`:

    {:user {:plugins [[lein-tarsier "0.9.1"]]}}

Now you can create a new project and run the plugin:

    $ lein new nailgun-test
    $ cd nailgun-test
    $ lein vimclojure
    Copying 2 files to /Users/dave/Documents/tmp/nailgun-test/lib
    Starting VimClojure server on 127.0.0.1, port 2113
    Happy hacking!

Alternatively, if you hate yourself, run the server manually with the jar in this install:

    $ cd .vim/lib
    $ java -cp server-2.3.0.jar:/path/to/clojure-x.y.z.jar vimclojure.nailgun.NGServer
    NGServer started on all interfaces, port 2113.

Note that in this case you have to provide the *absolute* path to a Clojure jar.

## Second Test - The REPL

Now that our nailgun server is running, start up vim again and run the `:ClojureRepl` command. The vim screen will split and you'll see a new Clojure REPL. Try it out. Tricks worth knowing:

* `,close` to exit, or just `<esc>:q`
* If you're cursor's in the middle of a command, hit `ctrl-enter` to execute the command without having to jump to the end of the line first
* `,st` gives the last stack trace

See vimclojure.txt for more info.

## Third Test - Code

Now try editing a file with the nailgun server running:

    $ cd
    $ vim .vim/piglatin.clj

Hit `\ef` to evaluate the file. VimClojure will open a split window to show the result:

    ; Use \p to close this buffer!
    #'user/vowel?
    #'user/pig-latin
    "ellohay"
    "imclojurevay"

Put your cursor on the `(set` on line 2 and hit `\lw`. VimClojure with show the docs for `set`.

# What's Next?

* Go read the vimclojure documentation! (You may find it here: `bundle/vimclojure-x.x.x/doc/clojure.txt`).
* If you've already got vim configuration, you should be able to basically copy this directory structure over yours. Then move the Pathogen and VimClojure settings from `vimrc.vim` to your `vimrc` file.
* A more elaborate setup is described [here](http://blog.darevay.com/2010/10/how-i-tamed-vimclojure/)
* Enable paredit.

# Paredit

Paredit performs structured editing of Clojure S-expressions. To enable, edit `vimrc.vim` and set the following:

    let g:paredit_mode = 1

The paredit documentation is here: `bundle/paredit-x.x.x/doc/paredit.txt`.

# ClojureScript
VimClojure doesn't have any particular support for ClojureScript, but you can at least enable syntax highlighting and stuff in `.cljs` files by adding this to your `vimrc` file:

    autocmd BufRead,BufNewFile *.cljs setlocal filetype=clojure

# Resources

* The [VimClojure google group](https://groups.google.com/group/vimclojure)
* Official [VimClojure source repository](https://groups.google.com/group/vimclojure)

# Windows Notes

Note if you're on Windows users:

* Replace `.vim` with `vimfiles`. The location of "HOME" may vary for you.
* Replace symlink `.vimrc` with `_vimrc` with contents `runtime vimrc.vim`. Or just move `vimrc.vim` to `_vimrc`.
* You might need to adjust the NailgunClient setting in vimrc.vim

