---
title: "Beware ujson.dumps() Precision"
date: 2018-04-16
tags: ["python", "json"]
draft: false
---

Are you using [u(ltra)json](https://github.com/esnme/ultrajson) over the standard library's [json](https://docs.python.org/3/library/json.html) module due to the [performance benefits](https://github.com/esnme/ultrajson#benchmarks)?

Until recently, I was too, but then I discovered that `usjon.dumps()` loses a lot of "precision" in floats compared to `json.dumps()`:

```python
>>> ujson.dumps(1.3959197885670265)
'1.3959197886'

>>> json.dumps(1.3959197885670265)
'1.3959197885670265'
```

You might now be thinking to yourself, doesn't `usjon.dumps()` take a `double_precision`parameter that allows you to control the ["maximum digit precision of doubles"](https://github.com/esnme/ultrajson/blob/42044fef11c3cac897e71907fe9e63a4dcfecb91/python/ujson.c#L56)?

Well, you would be correct. Let's try it:

```python
>>> ujson.dumps(1.3959197885670265, double_precision=20)
'1.395919788567026'

>>> json.dumps(1.3959197885670265)
'1.3959197885670265'
```

OK, so `'1.395919788567026'` is definitely more "precise" than '1.3959197886' which was returned by the call without the `double_precision` argument. However, if you look closely you'll notice that even with `double_precision=20` there's still a single place of precision lost by `ujson.dumps()` compared to `json.dumps()`. This is despite the fact that setting `double_precision` to `20` should be more than enough to retain the full precision of the float in the example.

At the time of writing, I can't find a way to force `ujson.dumps()` to use arbitrary float precision. Because of this, I've switched back to plain old `json.dumps()` in the application where I originally discovered this issue, because not losing any precision is more important than performance.



