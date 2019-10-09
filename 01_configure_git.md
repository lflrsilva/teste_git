Git configuration
=================
Let's begin your Git package configuration. You may use `--global` if you want
the configuration for all Git operations. On the other hand, if you omit the
`--global` option, the configuration will be valid only for this repository.

Please mind that I'll keep `--global` option as I use the same account for 
every development project.

## Setting username and e-mail id
You can configure the username and e-mail in order to identify your actions on
the repository.

    $ git config --global user.name "Put your name here"
    $ git config --global user.email "your@email.com"

## Pretty colors :)
You want this. No more words.

    $ git config --global color.ui true
    $ git config --global color.status auto
    $ git config --global color.branch auto

## Setting default editor
Vim, for sure.

    $ git config --global core.editor vim

## Setting default merge tool
Vim again, for sure.

    $ git config --global merge.tool vimdiff

## Let's check our configuration
At last, we can check the configuration for Git operations.

    $ git config --list

