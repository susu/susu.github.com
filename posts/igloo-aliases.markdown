{
  title: "igloo's aliases",
  date:  "2013-01-15",
  description: ""
}

Todays I've discovered that in [igloo](http://igloo-testing.org/) you can use
alternate syntax for defining a context or a spec:

Until now I've written my tests similarly:

```c++

Context(an_XXX)
{
  Spec(it_should_blabla_if_method_is_called)
  {
    AssertThat(xxx.method(), Is().EqualTo("blabla"));
  }
  XXX xxx;
};
```

But from now:


```c++

Describe(an_XXX)
{
  It(should_blabla_if_method_is_called)
  {
    AssertThat(xxx.method(), Is().EqualTo("blabla"));
  }
  XXX xxx;
};
```

In my opinion it is more BDD-ish syntax.

**NOTE**: for this I had to include ```igloo/igloo_alt.h```

Anyway, you can find (or create) more alias in ```igloo/core/alternativeregistrationaliases.h```
