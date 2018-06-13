---
layout: post
title:  "Duck Typing"
date:   2018-06-06 15:23:55 -0500
categories: ruby
---
<style type="text/css">
  .right-image {
    float:right;
  }
</style>

![duck]({{ "assets/images/blog-duck.png"| absolute_url }}){: .right-image }

**What is Duck Typing?**

**Duck Typing is a technique that reduces costs.**

It does this by making code conform to the SOLID principles - specifically the Open/Closed Principle which states that an object should be open for extension but closed for modification.

In her book on Practical Object-Oriented Design in Ruby, Sandi Metz discusses how to implement Duck Types, and the benefits of doing so.

The premise is that once a duck type is implemented, several classes within an application will have a one or more similarly named methods defined within their public interface.

A benefit having such similarly named methods or **messages** is that other classes in the application can then depend on these messages as they are more stable, while remaining oblivious to the specifics of the objects that implement them.  

Resulting in the conclusion that **if an object quacks like a duck and walks like a duck then its class is immaterial, it <span style="color:#d14;">IS</span> a duck**

For an example of a duck type in action, we might look at a vacationer planning a vacation.  In this example, they have no idea of the exact type of destination being passed, but instead trust that the duck typed destination will implement the "book_flight" method, consequently telling the vacationer which airline they should book their flight on in order to plan their vacation.

```ruby
Vacationer.plan_vacation(destination)

class Vacationer
  def plan_vacation(destination)
    flight = destination.book_flight
  end
end

class BoraBora
  def book_flight
    "Air Tahiti"
  end
end

class Oslo
  def book_flight
    "Norwegian Air"
  end
end

class Wellington
  def book_flight
    "Qantas"
  end
end

class Mars
  def book_flight
    "Space Shuttle"
  end
end

vacation_destination = Mars.new
Vacationer.plan_vacation(vacation_destination)
```

Here we would expect the flight variable to now be "Space Shuttle". <span style="font-size:16px;">:rocket:</span>

As we can see, it would be very simple to add another destination that implemented the "book_flight" method to this list, making our application a cinch to use, while promoting ease of extendability and flexibility.

The BoraBora, Oslo, Wellington and Mars classes all achieve **polymorphism<sup>1</sup> by way of duck types**.

The example above makes it easy to see how we could also leverage inheritance to achieve a similar result.  If we have a super class named Destination that implemented both the "book_flight" and "pack_items" methods, and all subsequent destinations were to inherit from it, we would have **polymorphism by way of inheritance**.

**A caveat:** When implementing my first duck, I initially set the duck type up in my application to take in an array and return a string in one instance, and return a hash in the other. I then learned that duck types must take in the same kind of input and return the same kind of output to eliminate unnecessary complexity and work as expected.

Thus in Ruby, we can enable multiple classes to respond to the same messages. We can do this by leveraging Ruby’s features of polymorphism and dynamic typing<sup>2</sup>, both of which are touched upon below.  An object can therefore act like many interfaces, resulting in across-class types that are defined by behavior.<br/><br/>

<span style="color:#d14; font-size:16px; font-weight:bold;">1. POLYMORPHISM</span>

According to Sandi Metz, **Polymorphism in OOP is:**  

  * the ability of many different objects to respond to the same message.  
  * Senders of the message need not care about the class of the receiver.
  * A single message thus has many(poly) forms(morphs)

There are a few ways to implement polymorphism. Inheritance is one, duck typing is another.<br/><br/>

<span style="color:#d14; font-size:16px; font-weight:bold;">2. DYNAMIC VS STATIC TYPING</span>

  * Unlike dynamically-typed languages, most (though not all) statically-typed languages require that you declare the type of each variable and every method parameter.
  * Dynamic typing is in fact the very feature that paves the way for duck-typing.
  * If we knew the class of the object we were sending a message to, we would not need to send an across class message.
  * On the other hand, searching to create across class messages helps us to reveal stable abstractions, and areas where our code can be safely and easily extended in the future.<br/><br/>

<span style="font-size:12px;">
Metz, Sandi (2013). Practical Object-Oriented Design in Ruby: an agile primer. Upper Saddle River: Pearson Education, 2013. Print.
</span>
