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
<strong><span style="color:#900">WHAT IS IT?</span></strong> 
* In its simplest form, the <strong>Factory Method</strong> is a design pattern that encapsulates instantiation. 
* It defines an interface for instantiating objects.  
* The interface does not need to know the type of object it is required to instantiate.  
* The actual object it returns might be an instance of a subclass.
&nbsp;  
&nbsp;     

<strong><span style="color:#900;">THE FACTORY PATTERNS</span></strong>  

<strong>1. The Simple Factory</strong>  
The Simple Factory suggests replacing direct object creation (using a new operator) with a call to a special `factory` method. The constructor call should be moved inside that method. Objects returned by factory methods are often referred to as `products`.  The `Simple Factory` is the most basic implementation of the Factory Pattern.  

```ruby
FACTORY METHOD SETUP : DONUT CLASSES

module Donut
  def filling
  end

  def price
  end
end

class ChocolateDonut
  include Donut

  def initialize(size="regular")
    @size = size
  end

  def filling
    "chocolate"
  end

  def price
    "1.25"
  end
end

class JellyDonut
  include Donut

  def initialize(size="regular")
    @size = size
  end

  def filling
    "jelly"
  end

  def price
    "1.25"
  end
end

class PlainDonut
  include Donut

  def initialize(size="regular")
    @size = size
  end

  def filling
    "none"
  end

  def price
    "0.99"
  end
end
```
&nbsp;  
     
```ruby
SIMPLE FACTORY IMPLEMENTATION  

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
* The above method does not know that a chocolate donut will be made until runtime when it is passed a `type` of `:chocolate`.
* Our application allows the `make_donut` method to handle instantiation of a new donut.
* It can trust that any object that is instantiated and returned by `make_donut` will respond to the `.filling` and `.price` methods.
&nbsp;  
&nbsp;     

<strong>2. The Factory Method</strong>  
The Factory Method defines an interface for creating Factories that will each return an object by deferring to the Factory's subclasses on which type of object to instantiate. A superclass specifies all standard and generic behavior (using pure virtual "placeholders" for creation steps), and then delegates the creation details to subclasses that are supplied by the client.  

```ruby
FACTORY METHOD IMPLEMENTATION  

module DonutFactory
  def make_donut
    "placeholder for make_Donut implementation"
  end
end

class MiniDonutFactory
  include DonutFactory

  def make_donut(:type)
    if type == :chocolate
      ChocolateDonut.new("mini")
    elsif type == :jelly
      JellyDonut.new("mini")
    elsif type == :plain
      PlainDonut.new("mini")
    end
  end
end

class RandomDonutFactory
  include DonutFactory

  def make_donut
    type = [:chocolate, :jelly, :plain].sample

    if type == :chocolate
      ChocolateDonut.new
    elsif type == :jelly
      JellyDonut.new
    elsif type == :plain
      PlainDonut.new
    end
  end
end

mini_donut_factory = MiniDonutFactory.new
mini_donut_factory.make_donut(:chocolate)  

random_donut_factory = RandomDonutFactory.new
random_donut_factory.make_donut
```
* In the above example, multiple factories are created.
* Each `factory` returns a single element or `product`.
* Each `factory can encapsulate different computations` to return different products, such as a mini donut or a random donut.
* All `products` returned by each factory respond to the same methods, `filling` and `price`. 
&nbsp;  
&nbsp;     

<strong>3. The Abstract Factory</strong>  
The Abstract Factory defines an interface for creating Factories that will  each return multiple related objects without explicitly specifying their classes.  

```ruby
ABSTRACT FACTORY IMPLEMENTATION  

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
* Here too multiple factories are created.
* Each `factory` returns multiple elements or `products`; a `menu` and a `button`.
* Each set of elements created relates to each other, as in parts of a system.  
&nbsp;  
&nbsp;     

<strong><span style="color:#900;">SIMILARITIES TO THE TEMPLATE PATTERN</span></strong>  
&nbsp;    
The Template pattern and the Factory pattern are similar in that both leave the task of implementation to subclasses.
* In the Factory method, a superclass defines an interface to create an object. Subclasses decide which concrete class to instantiate. 
* In the Template Method pattern, a compile-time algorithm is selected by subclassing. 
* If instantiation is not involved, it is not a Factory. 
&nbsp;  
&nbsp;  

<strong><span style="color:#900;">TYPES OF INPUTS AND OUTPUTS</span></strong>  
* The Factory Pattern can take different types of inputs that might specify what type of object should be created and/or the parameters required by the instantiation methods it encapsulates, such as the `size` parameter in the case of `MiniDonuts`.
* However, it produces outputs of the same type, or different subtypes of the same type, e.g. different types of donuts. 
* Whether the outputs are of the same type or different subtypes, they must respond to the same messages, e.g. `.filling` and `.price`. 

&nbsp;  
<strong><span style="color:#900;">WHEN TO USE IT</span></strong>   
&nbsp;   
The Factory Method should be used when an application cannot predict the type of object required. In other words, it knows <strong>when</strong> a new object should be created but not <strong>what</strong> type.
