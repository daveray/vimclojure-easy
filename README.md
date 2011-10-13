# VimClojure - Easy Peasy Lemon Squeezy
This is a sample configuration for [VimClojure](http://www.vim.org/scripts/script.php?script_id=2501) to hopefully make the process of getting and using VimClojure more straightforward. It is basically a complete vim configuration with VimClojure and nothing else installed. It should be a good sanity check for setting everything up.

## Install
If you have an existing vim configuration, first move it out of the way. You can bring it back in after you feel like VimClojure's working for you.

    $ cd
    $ mv .vim .vim.bak
    $ mv .vimrc .vimrc.bak
    $ mv .gvimrc .gvimrc

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

Now we want to get to the serious stuff. We'll need to start a nailgun server to get the most use out of VimClojure. There are (at least) two options. If you're using `leiningen`, install the [lein-nailgun plugin] (https://github.com/ibdknox/lein-nailgun) (this is Chris Granger's fork, updated for 2.3.0), create a project, and run the plugin:

    $ lein plugin install org.clojars.ibdknox/lein-nailgun "1.1.1"
    $ lein new nailgun-test
    $ cd nailgun-test
    $ lein nailgun
    Copying 1 file to /.../nailgun-test/lib
    NGServer started on 127.0.0.1, port 2113.

or, run the server manually with the jar in this install:

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

* Go read [vimclojure.txt](https://github.com/daveray/vimclojure-easy/blob/master/bundle/vimclojure-2.3.0/doc/clojure.txt)!!!
* If you've already got vim configuration, you should be able to basically copy this directory structure over yours. Then move the Pathogen and VimClojure settings from `vimrc.vim` to your `vimrc` file.
* A more elaborate setup is described [here](http://blog.darevay.com/2010/10/how-i-tamed-vimclojure/)

# Resources

* The [VimClojure google group](https://groups.google.com/group/vimclojure)
* Official [VimClojure source repository](https://groups.google.com/group/vimclojure)

# Windows Notes

Note if you're on Windows users:

* Replace `.vim` with `vimfiles`. The location of "HOME" may vary for you.
* Replace symlink `.vimrc` with `_vimrc` with contents `runtime vimrc.vim`. Or just move `vimrc.vim` to `_vimrc`.
* You might need to adjust the NailgunClient setting in vimrc.vim

