---
layout: post
title: "Object Oriented Principles using C#"
categories: junk
author:
- Scott Harris
meta: "Development"
---

Here is a collection of my favorite object-oriented programming principles. While there are many, these are the ones I rely on the most.


### Principles of SOILD
SOLID principles are the design principles that enable us to manage several software design problems. Robert C. Martin compiled these principles in the 1990s. These principles provide us with ways to move from tightly coupled code and little encapsulation to the desired results of loosely coupled and encapsulated real business needs properly. SOLID is an acronym for the following.

- S: Single Responsibility Principle (SRP)
- O: Open-closed Principle (OCP)
- L: Liskov substitution Principle (LSP)
- I: Interface Segregation Principle (ISP)
- D: Dependency Inversion Principle (DIP)

#### Single Responsibility Principle (SRP)
The Single Responsibility Principle states that a class should have only one reason to change, meaning it should have just one responsibility. In other words, each class should focus on a single task or concern to make the code more maintainable and understandable.

##### Example:
{% highlight c# %}
// Not the best design - too many unrelated methods in a single interface
class AllInOneDevice {
  print() {
    // Print implementation
  }

  scan() {
    // Scan implementation
  }

  fax() {
    // Fax implementation
  }
}
// A much better design, adhering to the ISP
class Printer {
  print() {
    // Print implementation
  }
}

class Scanner {
  scan() {
    // Scan implementation
  }
}

class FaxMachine {
  fax() {
    // Fax implementation
  }
}

// Now, a class can implement only the interfaces it needs
class SimplePrinter extends Printer {
  print() {
    // Simple printing implementation
  }
}

class AllInOnePrinter extends Printer, Scanner, FaxMachine {
  print() {
    // Advanced printing implementation
  }

  scan() {
    // Advanced scanning implementation
  }

  fax() {
    // Advanced faxing implementation
  }
}
{% endhighlight %}

#### Open-Closed Principle (OCP)
https://www.designgurus.io/blog/essential-software-design-principles-you-should-know-before-the-interview
#### Liskov Substitution Principle (LSP)
#### Interface Segregation Principle (ISP)
#### Dependency Inversion Principle (DIP)

### Pricipals of DRY


