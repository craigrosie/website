---
title: "Getting PyCharm to work with pyenv local"
date: 2015-10-07
draft: true
---
I write the majority of my code in [Sublime Text][sublime], favouring it over a full blown IDE like [PyCharm][pycharm] or [Eclipse][eclipse]. However, I cannot argue with the debugging abilities that an IDE provides.

Recently I tried to use PyCharm's debugging functionality, and although when adding the folder as a PyCharm project and specifying the relevant [pyenv virtualenv][pyenv-virtualenv]<sup>1</sup> Python path (see image below), PyCharm just could not seem to find it.

![pyenv-add-project][pyenv-add-project]

After a bit of prodding and poking, I discovered that in the main PyCharm preferences, the Project Interpreter was still set to the pyenv global Python version, like so:

![pyenv-interpreter-3.4.3][pyenv-interpreter-3.4.3]

Changing this to use the pyenv virtualenv Python path for the project fixed things, and allowed me to debug my script.

![pyenv-interpreter-virtualenv][pyenv-interpreter-virtualenv]

This was definitely not an obvious fix (although admittedly I have very little PyCharm experience), so I hope it helps someone else out. I know I'll refer back to this when I inevitably run into the same issue again in ~6 months!

**Notes:**

1. If you want to learn more about [pyenv][pyenv] and [pyenv virtualenv][pyenv-virtualenv] and why they're **awesome**, you can check out my [previous post!][previous-post]



<!--- Links -->

[sublime]: http://www.sublimetext.com/3
[pycharm]: https://www.jetbrains.com/pycharm/
[eclipse]: https://eclipse.org/
[pyenv-add-project]: /img/2015-10-07-pycharm-with-pyenv/pycharm-add-project.png
[pyenv-interpreter-3.4.3]: /img/2015-10-07-pycharm-with-pyenv/pycharm-project-interpreter-3.4.3.png
[pyenv-interpreter-virtualenv]: /img/2015-10-07-pycharm-with-pyenv/pycharm-project-interpreter-virtualenv.png
[pyenv]: https://github.com/yyuu/pyenv
[pyenv-virtualenv]: https://github.com/yyuu/pyenv-virtualenv
[previous-post]: /setting-up-pyenv-virtualenv.html

