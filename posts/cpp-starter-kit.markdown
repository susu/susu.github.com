{
  title: "C++ Starter Kit",
  date:   "2012-12-9",
  description: "Posting using all supported markdown features"
}

I repeated many times my initial setup for any C++ project. I was
always re-creating the same files... *src, include, test directories,
integrating test framework into build system*...

Of course with time, it's getting better (to my taste)
(CMake, CxxTest, tup, igloo, ...). But these repetitions are violating
DRY. That's the main reason why I started to piece together a starter kit.

The other reason is to find a proper *mock framework*.
At the moment I'm playing with [turtle](http://turtle.sourceforge.net).

The repository can be reached [here](http://github.com/susu/cpp-starter-kit),
and the turtle integration is on a
[separate branch](https://github.com/susu/cpp-starter-kit/tree/turtle).

## Check it out!

<!-- **Be careful: this post may quickly became deprecated!** -->

### 1. Clone the repository

Of course the very first dependency is ```git```.

(if you don't have git, you can try it download as zip,
but then you miss the submodules and need to download them manually)

Because it's has some submodules (e.g. igloo), you need to clone it with ```--recursive```
```bash
$ git clone --recursive git://github.com/susu/cpp-starter-kit.git
```

In recursive mode git will also clone submodules, so you get my repo and all of
its submodule. That means you can update the submodule separately.
For example (in the toplevel of the working tree):
```bash
$ git submodule update test/igloo
```

### 2. Compile'em all!

Another important dependency is *tup*. Tup is a very fast build system.
I will not cover here the installation of tup, but for ArchLinux users:
it can be installed via [AUR packages](https://aur.archlinux.org/packages/tup-git/).

So we have tup, we have the cloned repo... let's build it.
First you need to initialize your tup directory in your working tree, and then build:
```bash
$ tup init
.tup repository initialized.
$ tup upd
```

Then you should see the nice coloured progress of the build.

This starter kit contains a dummy test example with a calculator

### 3. Addding files/classes to the project

The tup build system using ```Tupfile```-s as main files. There is no Tupfile in the
root, only in ```src/``` and in ```test/```. Only a ```Tuprules.tup``` file can be
found in the root. This file contains all common stuff for Tupfiles.

I don't want to deeply describe the tup system here, it has a pretty good
[documentation](http://gittup.org/tup/examples.html).

Let's see it in a more practical way: what if I want to add another class to the repo?

I want to add a ```Display``` class which can show the result. Of course we are working
in TDD, let's add test first: create a ```TestDisplay.hpp``` file in ```test/```
with the following content:

```c++

#include <igloo/igloo.h>

#include <Display.hpp>

Context(a_display)
{
  a_display()
    : display(output)
  {}

  void SetUp()
  {
    output.str("");
  }

  Spec(should_display_number)
  {
    display.show(123);
    Assert::That( output.str(), Contains("123"));
  }

  std::ostringstream output;
  Display display;
};


```

I will not cover igloo and BDD here. Context is something like test suites or test
fixtures in other unit testing frameworks, and Spec is something like testcase.
However to apply Context and Spec well, you may need to learn more about BDD.

- - -
##### A small explanation of the test

Display's constructor accepts an ```std::ostream``` as an argument, the
display will put numbers into. In the tests we are using an ```std::ostringstream```
for output, so it can be easily checked.
- - -

We've created the file, but not added to the system. In the ```test/``` directory there
is a ```test_runner.cpp```. You need to add an include:

```c++
#include "igloo/igloo.h"

#include "TestCalculator.hpp"
#include "TestDisplay.hpp" // added line

int main()
{
  return igloo::TestRunner::RunAllTests();
}
```

Now compile, it should fail, because Display is unknown. Let's add it!
(I will not show every step of TDD here, it isn't in focus right now.)

In ```include/``` dir:
```c++
#ifndef DISPLAY_HPP_INC
#define DISPLAY_HPP_INC

#include <ostream>

class Display
{
  public:
    Display(std::ostream & out);
    void show(int);
  private:
    std::ostream & m_out;
};

#endif
```

Now compile, you should get ```undefined reference``` error, because
definitions are missing:

In ```src/``` dir:
```c++
#include <Display.hpp>

Display::Display(std::ostream & out)
  : m_out(out)
{}

void Display::show(int result)
{
  m_out << result;
}
```

Note: all ```.cpp``` files in ```src/``` are built automatically.

## Final notes

 * You may noticed that it compiles a dynamic library, and there is no ```main()```
in the *so-called production code*, only in tests.
 * It's not integrates any mock framework (yet)

Anyway, feel free to fork it and modify to your taste!
(Also if you fix some error, please send me a pull request!)
