---
title: "Copy to the clipboard in Vim"
date: 2015-10-01
tags: ["vim"]
draft: false
---
I love using Vim for making quick changes to files when I'm working on the command line. I even use the Vim keyboard shortcuts in Sublime Text, and these days I struggle to use any text editor that does not allow me to enable Vim key bindings.

Recently, I found myself needing to copy text to the clipboard from a text file that I had open in Vim. I am pretty ashamed to say that I didn't think there'd be a key combination to allow me to do this, so I always resorted to using the mouse.

This was getting pretty tedious - I mean, the whole point of Vim is that you shouldn't ever need a mouse! *Obviously* there is a way to do it with the keyboard!

Here we go:

  ```
  "*y
  ```

Wait, what? Let's break that down:

* `"` - specify a register
* `*` - use the `*` (clipboard) register
* `y` - standard Vim yank command

**Note:** You will need to have a section of text visually selected to use this command (i.e. by using the `v` Vim command)

You can also paste in a similar way:

  ```
  "*p
  ```

More information can be found in [this StackOverflow thread][so-thread]


<!--- Links -->

[so-thread]: http://stackoverflow.com/questions/3961859/how-to-copy-to-clipboard-using-vim
