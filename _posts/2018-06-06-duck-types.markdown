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

What we want is to have our application respond to something along the lines of:  

<span style="color:#1a92bb;">**Vacationer.plan_vacation(destination)**</span><br /><br />


```ruby
class Vacationer
  def plan_vacation(destination)
    result = {}
    result[:flight] = destination.book_flight
    result[:items] = destination.pack_items
    result
  end
end

class BoraBora
  def book_flight
    'Air Tahiti'
  end

  def pack_items
    'shorts, swim things, spf'
  end
end

class Oslo
  def book_flight
    'Norwegian Air'
  end

  def pack_items
    'sweaters, winter socks'
  end
end

class Wellington
  def book_flight
    'Qantas'
  end

  def pack_items
    'sweaters, hiking shoes, sunglasses'
  end
end

class Mars
  def book_flight
    "SpaceX's BFR rocket system"
  end

  def pack_items
    'space suit, oxygen'
  end
end


# EXAMPLE I

vacationer_1 = Vacationer.new
first_vacation_destination = Mars.new
vacationer_1.plan_vacation(first_vacation_destination)

puts result[:flight] #=> 'SpaceX's BFR rocket system'
puts result[:items]  #=> 'space suit, oxygen'


# EXAMPLE II

vacationer_2 = Vacationer.new
second_vacation_destination = BoraBora.new
vacationer_2.plan_vacation(second_vacation_destination)

puts result[:flight] #=> 'Air Tahiti'
puts result[:items]  #=> 'shorts, swim things, spf'

```

As we can see, using the same **plan_vacation** method, in the first case we get the flight variable to be <span style="color:#1a92bb;">'SpaceX's BFR rocket system'</span><span style="font-size:16px;">:rocket:</span>, and in the second case we get a flight variable that's set to <span style="color:#1a92bb;">'Air Tahiti'</span>

And for the items variable, in the first case we get <span style="color:#1a92bb;">'space suit, oxygen'</span>, and in the second, <span style="color:#1a92bb;">'shorts, swim things, spf'</span>.

By now it's clear that it would be very simple to add another destination that implemented the <span style="color:#1a92bb;">book_flight</span> and <span style="color:#1a92bb;">pack items</span> methods to this list, making our application a cinch to use, while promoting extendability and flexibility.

The <span style="color:#1a92bb;">BoraBora</span>, <span style="color:#1a92bb;">Oslo</span>, <span style="color:#1a92bb;">Wellington</span> and <span style="color:#1a92bb;">Mars</span> classes all achieve **polymorphism<sup>1</sup> by way of duck types**.

The example above makes it easy to see how we could also leverage **inheritance** to achieve a similar result.  If we had a <span style="color:#1a92bb;">super class named Destination</span> that implemented both the <span style="color:#1a92bb;">book_flight</span> and <span style="color:#1a92bb;">pack_items</span> methods, and all subsequent destinations were to inherit from it, we would have **polymorphism by way of inheritance**.

**A caveat:** When implementing my first duck, I initially set the duck type up in my application to take in an array and return a string in one instance, and to return a hash in the other. I then learned that duck types must take in the same kind of input and return the same kind of output to eliminate unnecessary complexity, and to work as expected.

Returning different kinds of output would make our duck types less easy to depend on by creating more work for us to do to check the nature of the return values later, subsequently driving up costs. This would impact the goal of our duck types adversely!

Thus in Ruby, we can enable multiple classes to respond to the same messages. We can do this by leveraging Ruby’s features of polymorphism and dynamic typing<sup>2</sup>. Both of these are touched upon below.  An object can therefore act like many interfaces, resulting in across-class types that are defined by behavior.<br/><br/>

<span style="color:#d14; font-size:16px; font-weight:bold;">1. POLYMORPHISM</span>

According to Sandi Metz, **Polymorphism in OOP is:**  

  * the ability of many different objects to respond to the same message.  
  * Senders of the message need not care about the class of the receiver.
  * A single message thus has many(poly) forms(morphs)

There are a few ways to implement polymorphism. Inheritance is one, duck typing is another.<br/><br/>

<span style="color:#d14; font-size:16px; font-weight:bold;">2. DYNAMIC VS STATIC TYPING</span>

  * Unlike dynamically-typed languages, most (though not all) statically-typed languages require that you declare the type of each variable and every method parameter.
  * Dynamic typing is in fact the very feature that paves the way for duck-typing.
  * If we knew the class of the object we were sending a message to, we would not need to send an 'across class' message.
  * On the other hand, searching to create across class messages helps us reveal stable abstractions, and areas where our code can be safely and easily extended in the future.<br/><br/>

<span style="font-size:12px;">
Metz, Sandi (2013). Practical Object-Oriented Design in Ruby: an agile primer. Upper Saddle River: Pearson Education, 2013. Print.</span>

<span style="font-size:12px;">
Castillo, M (2018, March 11). Elon Musk, speaking at SXSW, projects Mars spaceship will be ready for short trips by first half of 2019 [online] Available at:
<https://www.cnbc.com/2018/03/11/elon-musk-says-mars-spaceship-will-be-ready-for-short-trips-by-first-half-of-2019.html> [Accessed 14 Jun. 2018]</span>
