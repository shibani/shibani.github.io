---
layout: post
title:  "The Visitor Pattern"
date:   2018-07-29 15:23:55 -0500
categories: patterns
---

![game of thrones]({{ "assets/images/game-of-thrones.png"| absolute_url }})
&nbsp;  

<strong><span style="color:#900">WHAT IS IT?</span></strong>  

* The visitor pattern is a way to add methods to classes of different types with minimal altering of those classes.    
* It allows you to define external classes that can extend other classes without any major editing to them.  
* It leverages polymorphism and dynamic binding in order to achieve the desired results.
* Two parallel interfaces are created, involving double dispatch 
  * the visitor interface (Visitor, visit)     
  * the visitable interface (Visitable, accept)   
* You can create completely different methods depending on the class used
* This pattern promotes speed of extensibility.    

```ruby
# CLASS THAT WE WOULD LIKE TO EXTEND WITHOUT CHANGING  

class House 
  attr_reader :name

  def initialize(name)   
    @name = name
  end 
end
```

```ruby
# STEP 1. CREATE MODULES TO EXTEND THE CLASS

module Visitor
  def visit(object)  
    "override implementation here"
  end  
end

module Visitable 
  def accept(visitor)  
    visitor.visit(self)  
  end      
end
```

```ruby
# STEP 2. INCLUDE THE VISITABLE MODULE 
# IN THE CLASS THAT WE WOULD LIKE TO EXTEND

class House 
  include Visitable 
  attr_reader :name

  def initialize(name)   
    @name = name
  end
end
```

```ruby
# STEP 3. INCLUDE THE VISITOR MODULE 
# IN A NEW CLASS THAT ADDS THE BEHAVIOR WE WANT 

class AddPerson
  include Visitor
  attr_reader :first_name
  attr_reader :character
  
  def initialize(first_name, character, alive=true)
    @first_name = first_name
    @alive = alive
    @character = character
  end

  def alive?
    @alive
  end

  def get_person(visitable_object)
    status = alive? ? "Is alive" : "Is dead"
    puts "#{first_name} #{visitable_object.name}: #{character} guy"
    puts "#{status}\n\n"
  end

  def visit(visitable_object)         # IMPLEMENT THE VISIT METHOD 
    get_person(visitable_object)
  end
end
```

```ruby
arya = AddPerson.new("Arya", "good")
stark = House.new("Stark")
stark.accept(arya)          
#=> Arya Stark: good guy, Is alive

ned_stark = AddPerson.new("Ned", "good", false)
stark.accept(ned_stark)     
#=> Ned Stark: good guy, Is dead

tyrion = AddPerson.new("Tyrion", "good")
lannister = House.new("Lannister")
lannister.accept(tyrion)    
#=> Tyrion Lannister: good guy, Is alive 

cersei = AddPerson.new("Cersei", "bad")
lannister.accept(cersei)    
#=> Cersei Lannister: bad guy, Is alive

```  
&nbsp;  
&nbsp; 
<strong><span style="color:#900">PLAY AROUND WITH IT IN THIS REPL!</span></strong>
&nbsp; 
&nbsp; 
<iframe height="1200px" width="100%" src="https://repl.it/@shibani77/SomberDazzlingDownloads?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>