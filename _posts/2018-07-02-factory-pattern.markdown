---
layout: post
title:  "The Factory Pattern"
date:   2018-07-02 15:23:55 -0500
categories: patterns
---
<style type="text/css">
  html {
    scroll-behavior: smooth;
  }

  a{
    text-decoration:none;
  }

  a:hover, a:active, a:visited, a:focus{
    text-decoration:none;
  }

  ul.contents{
    margin:15px 0px 20px 20px;
    color:#2a7ae2;
  }

  .menu-item{
    font-size:16px;
    font-weight:bold;
    color:#0099ff; 
    color:#1a92bb;
    color:#2a7ae2;
  }

  li a .menu-item:hover{
    text-decoration:none !important;
    color:#0099ff; 
  }
</style>
![donuts]({{ "assets/images/donuts.jpg"| absolute_url }})
<p class="menu-item" style="margin-top:15px;">In this post:</p>
<ul class="contents"> 
  <li><a href="#what-is-it"><span class="menu-item">What is it?</span></a></li>  
  <li><a href="#simple-factory"><span class="menu-item">The Simple Factory</span></a></li>  
  <li><a href="#factory-method"><span class="menu-item">The Factory Method</span></a></li>  
  <li><a href="#abstract-factory"><span class="menu-item">The Abstract Factory</span></a></li> 
  <li><a href="#template-pattern"><span class="menu-item">Similarities to the Template Pattern</span></a></li>
  <li><a href="#inputs-outputs"><span class="menu-item">Types of inputs and outputs</span></a></li>
  <li><a href="#when-to-use"><span class="menu-item">When to use it</span></a></li>
</ul>  
<hr />   
&nbsp;  
<strong><span id="what-is-it" style="color:#900">WHAT IS IT?</span></strong>
* In its simplest form, the <strong>Factory Method</strong> is a design pattern that encapsulates instantiation.
* It defines an interface for instantiating objects.  
* The interface does not need to know the type of object it is required to instantiate.  
* The actual object it returns might be an instance of a subclass.
&nbsp;  
&nbsp;     

<strong><span id="simple-factory" style="color:#900;">THE FACTORY PATTERNS</span></strong>  

<strong>1. The Simple Factory</strong>  
The Simple Factory suggests replacing direct object creation (using a new operator) with a call to a special `factory` method. The constructor call should be moved inside that method. Objects returned by factory methods are often referred to as `products`.  The `Simple Factory` is the most basic implementation of the Factory Pattern.  

```ruby
# FACTORY METHOD SETUP : DONUT CLASSES

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
# SIMPLE FACTORY IMPLEMENTATION  

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

<span id="factory-method"><strong>2. The Factory Method</strong></span>  
The Factory Method defines an interface for creating Factories that will each return an object by deferring to the Factory's subclasses on which type of object to instantiate. A superclass specifies all standard and generic behavior (using pure virtual "placeholders" for creation steps), and then delegates the creation details to subclasses that are supplied by the client.  

```ruby
# FACTORY METHOD IMPLEMENTATION  

module DonutMaker
  def make_donut
    "placeholder for make_Donut implementation"
  end
end

class RandomDonutFactory
  include DonutMaker

  def make_donut
    flavor = [:chocolate, :jelly, :plain].sample

    if flavor == :chocolate
      ChocolateDonut.new
    elsif flavor == :jelly
      JellyDonut.new
    elsif flavor == :plain
      PlainDonut.new
    end
  end
end

class MiniDonutFactory
  include DonutMaker

  def make_donut(flavor)
    if flavor == :chocolate
      ChocolateDonut.new("mini")
    elsif flavor == :jelly
      JellyDonut.new("mini")
    elsif flavor == :plain
      PlainDonut.new("mini")
    end
  end
end

class DonutFactory
  def create_donut(type, flavor=nil)
    if type == :random
      random_donut_factory = RandomDonutFactory.new
      random_donut_factory.make_donut
    elsif type == :mini
      mini_donut_factory = MiniDonutFactory.new
      mini_donut_factory.make_donut(flavor)  
    end
  end
end

donut_factory = DonutFactory.new
donut_factory.create_donut(:random)
donut_factory.create_donut(:mini, :chocolate)
```
* In the above example, multiple factories are created.
* Each `factory` returns a single element or `product`.
* Each `factory can encapsulate different computations` to return different `products`, such as a `mini donut` or a `random donut`.
* The application does not know the type of `factory` or `product` that will be created until runtime.
* All `products` returned by each factory respond to the same methods, `filling` and `price`.
&nbsp;  
&nbsp;     

<span id="abstract-factory"><strong>3. The Abstract Factory</strong></span>  
The Abstract Factory defines an interface for creating Factories that will  each return multiple related objects without explicitly specifying their classes.  

```ruby
# ABSTRACT FACTORY IMPLEMENTATION  

module UI_Elements
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
  include UI_Elements

  def create_menu
    MacOSMenu.new
  end

  def create_button
    MacOSButton.new
  end
end

class WindowsFactory
  include UI_Elements
  def create_menu
    WindowsMenu.new
  end

  def create_button
    WindowsButton.new
  end
end

class IOSFactory
  include UI_Elements

  def create_menu
    IOSMenu.new
  end

  def create_button
    IOSButton.new
  end
end

class ApplicationUIFactory
  def create_ui_elements(type)
    if type == :mac
      mac_os_factory = MacOSFactory.new
      mac_os_factory.create_elements
    elsif type == :windows
      windows_factory = WindowsFactory.new
      windows_factory.create_elements
    elsif type == :ios
      ios_factory = IOSFactory.new
      ios_factory.create_elements
    end
  end
end

application_ui_factory = ApplicationUIFactory.new
application_ui_factory.create_ui_elements(:mac)   
```
* Here too multiple factories are created.
* Each `factory` returns multiple elements or `products`; a `menu` and a `button`.
* Each set of elements created relates to each other, as in parts of a system.  
* The above method does not know that a set of Mac OS elements will be created until runtime when it is passed a `type` of `:mac`.
* Our application allows the factory subtypes to handle instantiation of each new set of UI elements.
&nbsp;  
&nbsp;     

<strong><span id="template-pattern" style="color:#900;">SIMILARITIES TO THE TEMPLATE PATTERN</span></strong>  
&nbsp;    
The Template pattern and the Factory pattern are similar in that both leave the task of implementation to subclasses.
* In the Factory method, a superclass defines an interface to create an object. Subclasses decide which concrete class to instantiate.
* In the Template Method pattern, a compile-time algorithm is selected by subclassing.
* If instantiation is not involved, it is not a Factory.
&nbsp;  
&nbsp;  

<strong><span id="inputs-outputs" style="color:#900;">TYPES OF INPUTS AND OUTPUTS</span></strong>  
* The Factory Pattern can take different types of inputs that might specify what type of object should be created.
* However, it produces outputs of the same type, or different subtypes of the same type, e.g. different types of donuts.
* Whether the outputs are of the same type or different subtypes, they will respond to the same messages, e.g. `.filling` and `.price`.

&nbsp;  
<strong><span id="when-to-use" style="color:#900;">WHEN TO USE IT</span></strong>   
&nbsp;   
The Factory Method should be used when an application cannot predict the type of object required. In other words, it knows <strong>when</strong> a new object should be created but not <strong>what</strong> type.
