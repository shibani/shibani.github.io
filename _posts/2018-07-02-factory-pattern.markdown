---
layout: post
title:  "The Factory Pattern"
date:   2018-07-02 15:23:55 -0500
categories: js
---
<style type="text/css">
  .code{
    font-family:"Courier New", Courier, monospace;
  }
</style>

![donuts]({{ "assets/images/donuts.jpg"| absolute_url }})
&nbsp;  
<strong><span style="color:#666">WHAT IT IS</span></strong> 
* In its simplest form, the <strong>Factory Method</strong> encapsulates instantiation 
* It defines an interface for instantiating objects.  
* The interface does not need to know the type of object it is required to instantiate  
* It instead defers instantiation to subclasses. 
&nbsp;  
&nbsp;     

<strong><span style="color:#666">TYPES OF FACTORY PATTERNS</span></strong>   
&nbsp;  
<strong>1. The Simple Factory</strong>  
The Simple Factory suggests replacing direct object creation (using a new operator) with a call to a special `factory` method. The constructor call should be moved inside that method. Objects returned by factory methods are often referred to as `products`.  

```ruby
class DonutFactory
  def make_donut(type)
    if type == :chocolate
      ChocolateDonut.new
    elsif type == :jelly
      JellyDonut.new
    elsif type == :plain
      PlainDonut.new
    end
  end
end

donut_factory = DonutFactory.new
donut_factory.make_donut(:chocolate)   
```  
* The above method does not know that a chocolate donut will be made until runtime when it is passed a `type` of `:chocolate`
* It allows the `make_donut` method delegate instantion of the new donut to the donut subtypes 
* It does not need to know which subtype is responsible.
&nbsp;  
&nbsp;     

<strong>2. The Factory Method</strong>  
The Factory Method defines an interface for creating Factories that will each return an object by deferring to subclasses on which class to instantiate.  

```ruby
module DonutFactory
  def make_Donut
    "placeholder for make_Donut implementation"
  end
end

class MiniDonutFactory
  include DonutFactory

  def make_donut(:type)
    #=> will return a mini Donut
  end
end

class RandomDonutFactory
  include DonutFactory

  def make_donut
    #=> will return a random Donut
  end
end

mini_donut_factory = MiniDonutFactory.new
mini_donut_factory.make_donut(:chocolate)  

random_donut_factory = RandomDonutFactory.new
random_donut_factory.make_donut
```
* In the above example, multiple factories are created
* Each `factory` returns a single element or `product`
* Each `factory can encapsulate different computations` to return different products, such as a mini donut or a random donut
&nbsp;  
&nbsp;     

<strong>3. The Abstract Factory</strong>  
The Abstract Factory defines an interface for creating Factories that will  each return multiple related objects without explicitly specifying their classes.  

```ruby
module UI_Factory
  def create_menu
    "placeholder for menu implementation"
  end

  def create_button
    "placeholder for button implementation"
  end

  def create_elements
    create_menu
    create_button
  end
end

class MacOSFactory 
  include UI_Factory

  def create_menu
    MacOSMenu.new
  end

  def create_button
    MacOSButton.new
  end
end

class WindowsFactory
  include UI_Factory
  def create_menu
    WindowsMenu.new
  end

  def create_button
    WindowsButton.new
  end
end

class IOSFactory
  include UI_Factory

  def create_menu
    IOSMenu.new
  end

  def create_button
    IOSButton.new
  end
end

MacOSFactory.create_elements 
WindowsFactory.create_elements  
IOSFactory.create_elements  
```
* Here too multiple factories are created
* Each `factory` returns multiple elements or `products`; a `menu` and a `button`
* Each set of elements created relates to each other, as in parts of a system
&nbsp;  
&nbsp;     

<strong><span style="color:#666">TEMPLATE PATTERN</span></strong>  
The Template pattern is similar to the Factory pattern, except if instantiation is not involved, it is not a Factory.      

&nbsp;  
<strong><span style="color:#666">TYPES OF INPUTS AND OUTPUTS</span></strong>  
The Factory Pattern must take in the same types of inputs and return inputs of the same type (or different subtypes).      

&nbsp;  
<strong><span style="color:#666">WHEN TO USE IT</span></strong>  
When an application cannot predict the type of object required -- it knows when a new object should be created but not what type.
