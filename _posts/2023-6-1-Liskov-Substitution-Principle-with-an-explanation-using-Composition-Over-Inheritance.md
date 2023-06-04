---
layout: post
title: Liskov Substitution Principle with an explanation using Composition Over Inheritance
categories: [Liskov Substitution Principle, Composition Over Inheritance, SOLID, Clean Code, Object-Oriented Programming, Inheritance, Composition]
---


The Liskov Substitution Principle (LSP) is another important design principle in object-oriented programming. It’s named after Barbara Liskov, who introduced the concept in a 1987 paper. The idea behind LSP is that objects of a superclass should be able to be replaced with objects of a subclass without affecting the correctness of the program. In other words, if you have a piece of code that works with a superclass, you should be able to replace that superclass with any of its subclasses and the program will still work correctly.

This might sound a bit abstract, so let’s try to break it down with an example. Imagine we’re building a program to model different types of animals. We start with an Animal class that has some basic properties and methods that all animals share:

```
class Animal {
  constructor(name) {
    this.name = name;
  }

  eat(food) {
    console.log(`${this.name} is eating ${food}`);
  }

  sleep() {
    console.log(`${this.name} is sleeping`);
  }
}
```

Now, let’s say we want to create a subclass of Animal for a specific type of animal, like a Cat. We could do this using inheritance:

```
class Cat extends Animal {
  constructor(name) {
    super(name);
  }

  meow() {
    console.log(`${this.name} says meow`);
  }
}
```

In this case, Cat inherits all the properties and methods of Animal, but we’ve added a new method that’s specific to cats. This is a pretty straightforward example of inheritance, but it can actually lead to problems with the Liskov Substitution Principle.

For example, imagine we have some code that works with an Animal object to feed it and put it to sleep:

```
function feedAndSleep(animal) {
  animal.eat('some food');
  animal.sleep();
}
```

We could call this function with an instance of Animal or any of its subclasses, including Cat:

```
const animal = new Animal('Generic animal');
feedAndSleep(animal); // works fine

const cat = new Cat('Mr. Whiskers');
feedAndSleep(cat); // also works fine
```

This is all well and good, but now let’s say we add another subclass of Animal, like a RobotAnimal:

```
class RobotAnimal extends Animal {
  constructor(name) {
    super(name);
  }

  recharge() {
    console.log(`${this.name} is recharging`);
  }

  sleep() {
    console.log(`${this.name} is in standby mode`);
  }
}
```

In this case, RobotAnimal is still a subclass of Animal, but it behaves differently in some important ways. For example, instead of sleeping, it goes into standby mode and recharges. If we try to call our feedAndSleep function with a RobotAnimal, it will still work (since RobotAnimal inherits the eat method from Animal), but the sleep method won’t behave as expected:

```
const robot = new RobotAnimal('RoboDog');
feedAndSleep(robot); // prints "RoboDog is eating some food" and "RoboDog is in standby mode"
```

This is a problem because our feedAndSleep function assumes that any instance of Animal will behave in a certain way when it’s asleep. By breaking this assumption with the RobotAnimal class, we’ve violated the Liskov Substitution Principle.

So how can we fix this? One way is to use Composition Over Inheritance. Instead of creating a new subclass of Animal for each type of animal, we could create a separate class for each behavior that we want to add. For example, we could create a SleepBehavior class:

```
class SleepBehavior {
  sleep() {
    console.log('is sleeping');
  }
}
```

And then use this class to add sleeping behavior to our Animal and RobotAnimal classes:

```
class Animal {
  constructor(name) {
    this.name = name;
    this.sleepBehavior = new SleepBehavior();
  }

  eat(food) {
    console.log(`${this.name} is eating ${food}`);
  }

  sleep() {
    this.sleepBehavior.sleep();
  }
}

class RobotAnimal extends Animal {
  constructor(name) {
    super(name);
    this.sleepBehavior = new RobotSleepBehavior();
  }
}

class RobotSleepBehavior {
  sleep() {
    console.log('is in standby mode');
  }
}
```

Now, instead of using inheritance to add behavior, we’re using composition – we’re creating separate classes that can be combined to create different types of objects. When we create an instance of Animal or RobotAnimal, we also create an instance of SleepBehavior or RobotSleepBehavior, respectively. When we call the sleep method on these objects, it calls the sleep method of the sleep behavior object.

With this approach, we can still use the feedAndSleep function, but it works with any object that has a sleep behavior, regardless of its superclass:

```
function feedAndSleep(animal) {
  animal.eat('some food');
  animal.sleep();
}

const animal = new Animal('Generic animal');
feedAndSleep(animal); // works fine

const cat = new Animal('Mr. Whiskers');
cat.sleepBehavior = new CatSleepBehavior();
feedAndSleep(cat); // also works fine

const robot = new RobotAnimal('RoboDog');
feedAndSleep(robot); // works fine, prints "RoboDog is eating some food" and "is in standby mode"
```

By using Composition Over Inheritance, we’ve avoided violating the Liskov Substitution Principle. We can add new behaviors to our objects by creating new classes and composing them in different ways, without having to worry about the behavior of other objects in the hierarchy. This leads to more flexible, modular, and maintainable code.
