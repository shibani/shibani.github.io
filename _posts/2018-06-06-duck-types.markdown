---
layout: post
title:  "Duck Typing"
date:   2018-06-06 15:23:55 -0500
categories: ruby
---
**What is Duck Typing?**  

In her book on Practical Object-Oriented Design in Ruby, Sandi Metz discusses the advantages of Duck Typing.   

**Duck Typing is a technique that reduces costs.**

It does this by making code conform to the SOLID principles - specifically the Open/Closed Principle which states that an object should be open for extension but closed for modification.

As we work on moving away from focusing on classes and towards focusing on messages which are at the design center of our application, a further step in this direction is to **combine rigorously defined public interfaces with these messages**.  

When many of these rigorously defined public interfaces respond to the **same messages**, we create Duck Types.  Thus when a Command Line Interface object's public interface and a Browser object's public interface both implement and respond to a 'print' message, we are creating a Duck Type.

In Ruby we can enable multiple classes to respond to the same messages by leveraging Ruby’s features of dynamic typing and polymorphism, both of which are touched upon below.  An object can therefore act like many interfaces, resulting in across-class types that are defined by behavior:  

**If an object quacks like a duck and walks like a duck then its class is immaterial, it *IS* a duck**

In other words, applications may define many public interfaces that are not related to one specific class. Or you could say, these interfaces cut across class.  Using Duck Types makes code easily extendable. It becomes more flexible and easy to change.

**Polymorphism in OOP is:**
  * the ability of many different objects to respond to the same message.  
  * Senders of the message need not care about the class of the receiver.
  * A single message thus has many(poly) forms(morphs)

There are a few ways to implement polymorphism. Inheritance is one, duck typing is another.


**Dynamic vs. Static Typing**  

  * Unlike dynamically-typed languages, most (though not all) statically-typed languages require that you declare the type of each variable and every method parameter.
  * Dynamic typing is in fact the very feature that paves the way for duck-typing.
  * If we knew the class of the object we were sending a message to, we would not need to send an across class message.
  * On the other hand, searching to create across class messages helps us to reveal stable abstractions, and areas where our code can be safely and easily extended in the future.

<span style="font-size:12px;">
Metz, Sandi (2013). Practical Object-Oriented Design in Ruby: an agile primer. Upper Saddle River: Pearson Education, 2013. Print.
</span>
