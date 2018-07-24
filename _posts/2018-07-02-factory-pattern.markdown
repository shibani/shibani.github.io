---
layout: post
title:  "The Factory Pattern"
date:   2018-08-08 15:23:55 -0500
categories: js
---
<style type="text/css">
  .code{
    font-family:"Courier New", Courier, monospace;
  }
</style>

![donuts]({{ "assets/images/donuts.jpg"| absolute_url }})
&nbsp;  
<strong>WHAT IT IS</strong>  
* The Factory Pattern defines an interface for instantiating objects.  
* The interface does not need to know the type of object it is required to instantiate  
* It instead defers instantiation to subclasses.  

```ruby
class DonutFactory
  def make_donut(type)
    if type == :chocolate
      ChocolateDonut.new
    elsif type == :jelly
      JellyDonut.new
    else
      PlainDonut.new
    end
  end
end

DonutFactory.make_donut(:chocolate)
```

The Factory Method pattern suggests replacing direct object creation (using a new operator) with a call to a special "factory" method. The constructor call should be moved inside that method. Objects returned by factory methods are often referred to as "products."  

<strong>TEMPLATE PATTERN</strong>  
The Template pattern is similar to the Factory pattern, except if instantiation is not involved, it is not a Factory.  

<strong>TYPES OF INPUTS AND OUTPUTS</strong>  
The Factory Pattern must take in the same types of inputs and return inputs of the same type (or different subtypes).  

<strong>TYPES OF FACTORY PATTERNS</strong>  
The Simple Factory  
The Factory Pattern  
The Abstract Factory  

<strong>WHEN TO USE IT</strong>  
When an application cannot predict the type of object required -- it knows when a new object should be created but not what type.
