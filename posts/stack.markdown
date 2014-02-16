{
  title: "Data structures: stack",
  date: "2014-02-16"
}

Stack is one of the most fundamental data structure. It is a sequential data structure, it keeps the order of its
elements.

It has two simple operation:

 * **push** - adds an element to front
 * **pop**  - removes an element from front

```
       +---+---+---+
push-> |   |   |   |
<-pop  |   |   |   |
       +---+---+---+
```

Like when you put papers to your desk on each other. The latest is always on the top.

<center>![IMAGE: Papers on desk](/images/stack_desk.jpg)</center>

An example use case:
```
# Initial stack, empty:
{}

push(42):
{42}
 ^

push(15):
{15, 42}
 ^

push(99):
{99, 15, 42}
 ^

pop():
removes 99:
{15, 42}
 ^

pop():
removes 15:
{42}
 ^
```

Of course, from practical point of view, it has ```isEmpty()``` and may have ```getFront()``` or ```getTop()``` which
returns the current front element.


Let's see the implementation.

### Tests/Behaviour:

https://github.com/susu/data-struct-and-algo/blob/master/test/Stack.cpp

### Implementation (it is highly commented, I will not describe it here again):

https://github.com/susu/data-struct-and-algo/blob/master/include/ds/Stack.hpp

(Check the final source code [here](https://github.com/susu/data-struct-and-algo))
(You will need a compiler with C++11 support (GCC 4.8 or clang 3.3))

