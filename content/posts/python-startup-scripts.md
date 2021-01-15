---
title: "(I)Python Startup Scripts"
date: 2021-01-15
tags: ["python", "ipython", "command line"]
draft: false
---
<!-- vim-markdown-toc GFM -->

* [Python](#python)
* [IPython](#ipython)
* [But why?](#but-why)
    * [Basic Imports](#basic-imports)
    * [Custom Functions](#custom-functions)
    * [Taking it to the Next Level](#taking-it-to-the-next-level)

<!-- vim-markdown-toc -->

Are you a frequent user of Python or [IPython][ipython] interactive shells? Do you find yourself constantly reimporting the same libraries (_I'm looking at you_ `from pprint import pprint` 👀), or redefining the same helper functions?

This post will describe how to set up some scripts that will be automatically executed when you run `python` or `ipython` in your terminal.

## Python ##

When the Python interpreter starts, it looks for a [PYTHONSTARTUP][pythonstartup] environment variable. This can be set to point to a Python file which will be executed before you're dropped into the Python interpreter.

So if you create a `~/.custom_startup.py` with:
```python
print("hello from custom_startup.py!")
```

Then execute `export PYTHONSTARTUP=~/.custom_startup.py`.

Running `python` in your terminal, you'll now see `hello from custom_startup.py` printed before the `>>>` prompt in the interpreter:
```bash
$ python
Python 3.9.0 (default, Dec  1 2020, 10:32:01)
[Clang 12.0.0 (clang-1200.0.32.21)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
hello from custom_startup.py
>>>
```

## IPython ##

The [process for IPython is slightly different][ipython-startup-blog] - there's no environment variable to set, and IPython expects startup scripts (yes, that's intentionally plural) to live in `~/.ipython/profile_default/startup/`. There's even a `README` in that folder with some brief instructions:
```bash
$ cat ~/.ipython/profile_default/startup/README
This is the IPython startup directory

.py and .ipy files in this directory will be run *prior* to any code or files
specified via the exec_lines or exec_files configurables whenever you load this profile.

Files will be run in lexicographical order, so you can control the execution order of files
with a prefix, e.g.::

    00-first.py
    50-middle.py
    99-last.ipy
```

The keen-eyed amongst you will also have noticed the `profile_default` in the path above. This means you can have separate startup scripts for different IPython profiles. This is not a feature I use currently and therefore I won't cover it in more detail in this blog, but if you're interested then check out the [IPython docs][ipython-profiles].

## But why? ##

Now that you've created your (I)Python startup scripts, you can start adding some useful imports, custom functions or taking your Python interpreter experience to a whole new level.

### Basic Imports ###

As mentioned in the intro, do you find yourself always using `pprint` to print out data structures in a nicer format? Or maybe you're always using the `os` or `math` modules? Add these as imports to your startup script, and they'll be immediately accessible in your Python interpreter.

```python
import math
import os
from pprint import pprint
```
_~/.custom_startup.py or ~/.ipython/profile_default/startup/00-custom.py_

```python
$ python
Python 3.9.0 (default, Dec  1 2020, 10:32:01)
[Clang 12.0.0 (clang-1200.0.32.21)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> math
<module 'math' from '/Users/craig.rosie/.pyenv/versions/3.9.0/lib/python3.9/lib-dynload/math.cpython-39-darwin.so'>
>>> os
<module 'os' from '/Users/craig.rosie/.pyenv/versions/3.9.0/lib/python3.9/os.py'>
>>> pprint
<function pprint at 0x1020afc10>
```
_Look ma, no import errors!_

### Custom Functions ###

Maybe you find yourself writing the same helper functions over and over again. Or paging through your interpreter history trying to find the last time you wrote them. Wouldn't it be great if they were just always available?

```python
# previous imports excluded for brevity

def dir_no_dunder(module):
    """Like the dir() builtin, but only prints non-dunder attributes."""
    pprint([attr for attr in dir(module) if not attr.startswith("_")])
```
_~/.custom_startup.py or ~/.ipython/profile_default/startup/00-custom.py_
```python
$ python
Python 3.9.0 (default, Dec  1 2020, 10:32:01)
[Clang 12.0.0 (clang-1200.0.32.21)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> dir_no_dunder(math)
['acos',
 'acosh',
 'asin',
 'asinh',
 'atan',
 'atan2',
 'atanh',
 ...
```

### Taking it to the Next Level ###

Now for something that will give your interpreter superpowers. The [Rich][rich] library by [Will McGugan][willmcgugan] is an excellent addition to your startup script:

```python
...
from functools import partial
from rich import pretty, print, inspect

pretty.install()

inspectm = partial(inspect, methods=True)
```
_~/.custom_startup.py or ~/.ipython/profile_default/startup/00-custom.py_

For a start, you get nicely coloured and formatted output of data structures:

![image](https://user-images.githubusercontent.com/6367914/104753956-955f8f00-5750-11eb-9814-696562a05feb.png)

But perhaps _the_ killer feature is `inspect()`:

![image](https://user-images.githubusercontent.com/6367914/104754236-e53e5600-5750-11eb-91c0-b5e1715d37c9.png)

We can also create our own custom version of `inspect()`, which I've done using a [partial][partial], to give a function that shows you all the methods for a module or object: `inspectm = partial(inspect, methods=True)`

![image](https://user-images.githubusercontent.com/6367914/104754658-64cc2500-5751-11eb-83a6-6230833c6fd7.png)

Enjoy your interpreter with your new-found superpowers! 🎉

[ipython]: https://ipython.org/
[ipython-profiles]: https://ipython.org/ipython-doc/stable/config/intro.html#profiles
[rich]: https://github.com/willmcgugan/rich
[willmcgugan]: https://twitter.com/willmcgugan
[partial]: https://docs.python.org/3/library/functools.html#functools.partial
[pythonstartup]: https://docs.python.org/3/using/cmdline.html#envvar-PYTHONSTARTUP
[ipython-startup-blog]: https://switowski.com/blog/ipython-startup-files
