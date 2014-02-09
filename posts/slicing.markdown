{
  title: "Slicing",
  date: "2013-02-11",
  description: ""
}

C++ has many pitfall. Developers should be aware of them.
One of the pitfalls is **slicing.**
Instead of giving a proper definition, I show it by example:

```
...
class Animal
{
  public:
    virtual void eat()
    {
      std::cout << "Animal: eat!" << std::endl;
    }
};

class Dog : public Animal
{
  public:
    virtual void eat()
    {
      std::cout << "Dog: wrrrr, yummm!" << std::endl;
    }
};


// ... a few lines later ...

Dog frakk;
frakk.eat();

Animal anim = frakk;
anim.eat();
```

Output of the code:
```
Dog: wrrrr, yummm!
Animal: eat!
```

Whaat? Where we have created an ```Animal``` instance?
Well, yes we did. When we created the ```anim``` variable the ```frakk``` has been
implicitly casted to Animal. ([What](http://en.wikipedia.org/wiki/Frakk) is frakk
anyway?)

Then you may say: "okay, but why the heck should I declare a Dog and then copy it to
Animal?" And you are right. But check the following:

```
class Person
{
  public:
    void feed(Animal a)
    {
      a.eat();
    }
};

...
Dog frakk;

Person me;
me.feed(frakk);

```

Or, even imagine it in a more messy context ...

Okay, I don't want to let it happen. Let's delete the Animal's constructor with Dog
parameter (C++11. In C++03 you could make it private, and not give a definition).

```
class Dog;

class Animal
{
  public:
    Animal() = default;
    Animal(Dog&) = delete;
    virtual void eat()
    {
      std::cout << "Animal: eat!" << std::endl;
    }
};

class Dog : public Animal
{
  public:
    virtual void eat()
    {
      std::cout << "Dog: wrrrr, yummm!" << std::endl;
    }
};
```

Cool, now my compiler is claiming about:

```
slicing.cpp:47:15: error: use of deleted function ‘Animal::Animal(Dog&)’
slicing.cpp:9:5: error: declared here
slicing.cpp:28:10: error:   initializing argument 1 of ‘void Person::feed(Animal)’
```

Now, add a new animal to my farm:

```
class Cat
{
  public:
    virtual void eat()
    {
      std::cout << "Mmmm, fine little mice!" << std::endl;
    }
};
```

Oh-oh, but to avoid slicing with ```Cat``` I need to open my base class:

```
class Animal
{
  public:
    Animal() = default;
    Animal(Dog&) = delete;
    Animal(Cat&) = delete;
...
```

Bad news, and don't do that, because it violates the
[Open-Closed](http://en.wikipedia.org/wiki/Open/closed_principle) principle.

(Or, imagine if you derive from a base class which is part of a framework, and you cannot
modify it.)

So then, what should I do in order to avoid slicing?

If you can modify the base class, you can disable every copying:

```
class Animal
{
  public:
    Animal() = default;
    Animal(Animal&) = delete;
...
```

And if you still need to copy it, you can add a clone() function, which will do the
copying... but it will be in another post...

