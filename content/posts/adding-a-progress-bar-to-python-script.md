---
title: "Adding a progress bar to your Python script"
date: 2015-10-12
tags: ["python", "tqdm"]
draft: false
---

#### The problem?

You're writing a Python script which carries out some tasks in a loop. Maybe you're processing data in a bunch of files, or scraping a list of websites. Whatever it is, the task takes anything from a few seconds to a few minutes to complete. Wouldn't it be great if you could visualise progress (in the command line no less!) easily, and without some ugly `print()` statements in your code?

#### The solution!

Enter [tqdm][tqdm]! Lets create a trivial example to show the functionality:

```python
import time
from tqdm import *

for x in tqdm(range(10)):
    time.sleep(1)
```

Voila! Progess (bar)!

![tqdm][tqdm-gif]

The great thing about [tqdm][tqdm] is that is can take any iterable, and also has a bunch of options, which you can find out more about in the [tqdm source code][tqdm-doc].

**Notes:**

1. If you want to learn more about [pyenv][pyenv] and [pyenv virtualenv][pyenv-virtualenv] and why they're **awesome**, you can check out my [previous post!][previous-post]
2. The code used to generate the gif above actually uses the following code when generating the loop: `for x in tqdm(range(10), leave=True)`, as by default the progress bar will be 'erased' when the iterable terminates. Find out more [here][tqdm-doc].



<!--- Links -->

[tqdm]: https://github.com/tqdm/tqdm
[tqdm-gif]: https://cloud.githubusercontent.com/assets/6367914/10430121/5ceba8f8-70f5-11e5-80ea-48741dca109c.gif
[tqdm-doc]: https://github.com/tqdm/tqdm#documentation
[pyenv]: https://github.com/yyuu/pyenv
[pyenv-virtualenv]: https://github.com/yyuu/pyenv-virtualenv
[previous-post]: /blog/setting-up-pyenv-virtualenv/
