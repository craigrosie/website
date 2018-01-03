---
title: "Setting up pyenv and pyenv-virtualenv"
date: 2015-10-04
draft: true
---
I have a confession to make; I've only been using Python virtualenvs for about 6 months (I've been working with Python for ~2 years now).

To be honest, by the time I'd been introduced to them, I'd already royally screwed up my default Python's `site-packages`, *and* I was developing on Windows, so virtualenvs just seemed like a lot of hassle. Anyway, everything mostly seemed to sort of work, so why change anything, *right*?

**Wrong.** However much hassle, you might think virtualenvs are, spending hours debugging some obscure issue because you and your colleague have *ever-so-slightly* different versions of some third-party Python library will always be worse.

#### pyenv

[pyenv][pyenv] is a life-saver if you have to work with multiple versions of Python. Even if you don't, I'd recommend installing it, as it makes installing new versions of Python insanely easy. How easy you ask?

```bash
$ pyenv install 3.5.0
```

Now that you've installed a new version, [pyenv][pyenv] also makes it easy to change the default version used on your system:

```bash
$ pyenv global 3.5.0
```

You can even set multiple global versions at once:

```bash
$ pyenv global 2.7.10 3.5.0
$ python --version
Python 2.7.10
$ python2 --version
Python 2.7.10
$ python3 --version
Python 3.5.0
```

**Note:** whichever version you specify first will take precedence when you just type `python`.

[pyenv][pyenv] also has a `local` command, but I'll get onto that in a minute...

#### pyenv-virtualenv

[pyenv-virtualenv][pyenv-virtualenv], as you might have guessed, combines excellently with [pyenv][pyenv]. Creating a new virtualenv is as easy as:

```bash
$ pyenv virtualenv 3.5.0 project-virtualenv.3.5.0
```

where `3.5.0` is the version of Python that you want to use (that you've already installed with [pyenv][pyenv]!), and `project-virtualenv.3.5.0` is the name of your new virtualenv. I find appending the Python version to the end of the virtualenv name is a good habit to get into, as I never to work out which version of Python I'm currently using.

Which leads me on to my next point; [pyenv][pyenv] now treats this new virtualenv as a version of Python (you should see it listed if you run `pyenv versions`). Combining this with [pyenv][pyenv]'s ability to set `local` versions, you can have [pyenv][pyenv] *automatically activate* the correct virtualenv when you `cd` into the directory.

#### Automate all the things!

As easy as [pyenv][pyenv] and [pyenv-virtualenv][pyenv-virtualenv] make it create new virtualenvs, the automation can still be taken one step further. A lot of my work at the moment involves writing ad-hoc scripts to do some data-processing, creating a super-quick POC, or helping out colleagues who are using Python 2.7, and repeatedly typing out

```bash
$ mkdir ad-hoc-project
$ cd ad-hoc-project
$ pyenv virtualenv 2.7.10 ad-hoc-project.2.7.10
$ pyenv local ad-hoc-project.2.7.10
$ pip install pylint  # for Python linting in Sublime Text
$ pip install -r requirements.txt  # Sometimes I'd be working on an existing repo
```

got a bit tedious. I quickly realised  I could automate this with a bash function:

```bash
function pynew() {
    mkdir -p "$1" && cd "$1" &&  # passing `-p` means it doesn't fail if the dir exists
    pyenv virtualenv "$2" "$1"-"$2" &&  # create the new virtualenv
    pyenv local "$1"-"$2" &&  # set the new virtualenv to be the local Python version
    pipup &&  # a bash alias for pip install --upgrade pip
    pip install pylint &&  # for Python linting in Sublime Text
    [ -e "requirements.txt" ] &&  # check if requirements.txt exists...
    pip install -r requirements.txt  # ...and if it does, install it
}
```

We can call the new bash function like so:

```bash
pynew ad-hoc-project 2.7.10
```

Success!

##### Notes

* You can find installation instructions for [pyenv here][pyenv-install] and p[yenv-virtualenv here][pyenv-virtualenv-install].
* For more handy bash functions and aliases, you can check out my [prefs][prefs] GitHub repo. I'll be doing a blog post on this soon.



<!--- Links -->

[pyenv]: https://github.com/yyuu/pyenv
[pyenv-virtualenv]: https://github.com/yyuu/pyenv-virtualenv
[pyenv-install]: https://github.com/yyuu/pyenv#installation
[pyenv-virtualenv-install]: https://github.com/yyuu/pyenv-virtualenv#installation
[prefs]: https://github.com/craigrosie/prefs
