---
layout: post
title: "Building a simiple REST api in PeopleSoft"
categories: junk
author:
- Scott Harris
meta: "Development"
---

I have to give credit where credit is due this is a great resource for building REST Apis in PeopleSoft

(cedarhillsgroup)[https://ib.books.cedarhillsgroup.com/rest/intro-to-rest/]


Following this article I was able to create a prototype REST api using pagataion and with search.  Its a little specfici to oracle database but work pretty well.





### Pagatition
SOLID principles are the design principles that enable us to manage several software design problems. Robert C. Martin compiled these principles in the 1990s. These principles provide us with ways to move from tightly coupled code and little encapsulation to the desired results of loosely coupled and encapsulated real business needs properly. SOLID is an acronym for the following.

[designgurus](https://www.designgurus.io/blog/essential-software-design-principles-you-should-know-before-the-interview)

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
The Open-Closed Principle states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. In other words, you should be able to add new functionality to a class without modifying its existing code, by extending it or using other mechanisms like composition.

##### Example:
{% highlight c# %}
{% endhighlight %}

#### Liskov Substitution Principle (LSP)
The Liskov Substitution Principle states that objects of a derived class should be able to replace objects of the base class without affecting the program's correctness. In other words, derived classes should adhere to the behavior and contracts defined by the base class.

##### Example:
{% highlight c# %}
{% endhighlight %}


#### Interface Segregation Principle (ISP)
The Interface Segregation Principle states that clients should not be forced to depend on interfaces they do not use. In other words, large interfaces should be split into smaller, more specific ones so that a class implementing the interface only needs to focus on methods that are relevant to its functionality.

##### Example:
{% highlight c# %}
{% endhighlight %}


#### Dependency Inversion Principle (DIP)

### Pricipals of DRY

### Pricipals of KISS

### Pricipals of KISS


