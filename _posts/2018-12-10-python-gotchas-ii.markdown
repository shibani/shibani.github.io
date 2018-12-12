---
layout: post
title:  "Some Glitches and Gotchas 
encountered while writing Tictactoe in Python"
date:   2018-12-08 15:23:55 -0500
categories: python
---
<style type="text/css">
  .first-example{
    margin-top:40px;
    margin-bottom:40px;
    display: flex;
    justify-content:space-evenly;
  }
  .clear{
    clear:both;
  }
  .second-example{
    margin-top:40px;
    margin-bottom:40px;
    display: flex;
    justify-content:space-evenly;
  }
  .mutable-immutable-types{
    margin-top:40px;
    margin-bottom:40px;
  }
  blockquote {
    color: mediumpurple;
    border-left: 4px solid #e8e8e8;
    padding-left: 15px;
    font-size: 16px;
    letter-spacing: 0px;
    font-style: italic;
    font-weight: bold;
  }
  table tr:nth-child(even) {
    background-color: #fff;
  }
  .first-example table th, .first-example table td,
  .second-example table th, .second-example table td{
    padding: 0px 0px !important;
  }
  .mutable-immutable-types table th {
    background-color: #fff;
    border: 1px solid #666;
    border-bottom-color: #666;
  }
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

  .right-image {
    float:right;
    margin-left:20px;
    width:400px;
  }
</style>
<hr />  
<p class="menu-item" style="margin-top:15px;">In this post:</p>
<ul class="contents"> 
  <li><a href="#pass-by-reference"><span class="menu-item">Pass by Reference</span></a></li>  
  <li><a href="#none"><span class="menu-item">None in Python</span></a></li>  
  <li><a href="#repl-1"><span class="menu-item">Repl</span></a></li>  
  <br />
  <li><a href="#python-scope"><span class="menu-item">Python Scope</span></a></li>  
  <li><a href="#repl-2"><span class="menu-item">Repl</span></a></li>  
  <br />
  <li><a href="#unbound-local-variable"><span class="menu-item">Unbound Local Error</span></a></li> 
  <li><a href="#repl-3"><span class="menu-item">Repl</span></a></li>  
  <br />
  <li><a href="#mutable-default-args"><span class="menu-item">Mutable Default Arguments</span></a></li>  
  <li><a href="#repl-4"><span class="menu-item">Repl</span></a></li> 
  <li><a href="#mutable-immutable-types"><span class="menu-item">Mutable and Immutable Types in Python</span></a></li>  
</ul> 
<hr />  
&nbsp;   
<span style="color:#900; font-weight:bold; text-transform:uppercase;">Some Gotchas I uncovered while building Tictactoe in Python</span>  

My most recent project was writing Tictactoe in Python, a task I thought I had a reasonably clear idea of, after having written Tictactoe in both Ruby and Elixir.  I was thus very surprised to uncover hurdles that seemed to have absolutely no clear solution. Luckily our mentors are excellent at just this sort of thing and mine directed me to read up on... **Python Gotchas**.
<hr />  
&nbsp;  
<span id="pass-by-reference" style="color:#900; font-weight:bold; text-transform:uppercase;">PASSING BY REFERENCE</span>  

However, since `pass by reference` is not really a gotcha, I guess you could put this first one in the glitch category. I encountered this issue when I first tried to play the game loop. My game was able to ask the user for input, set the size of the board, create a board, and accurately create a human and computer player. However when I tried to place the first move, my move was not marked in the one square I had picked, but in 3 squares.

My game was marking the entire column.  

In other words, instead of looking like the image on the left below, my board looked like the image on the right. 

<div class="first-example">
  <div class="top-left">
    <table style="font-size:20px;text-align:center;" cellpadding="0" cellspacing="0" border="1px solid black">
      <colgroup>
        <col width="50px" />
        <col width="50px" />
        <col width="50px" />
      </colgroup>
      <tbody>
        <tr>
          <td style="height:50px; border:1px solid #000;font-weight:bold;"> x </td>
          <td style="height:50px; border:1px solid #000;"> 2 </td>
          <td style="height:50px; border:1px solid #000;"> 3 </td>
        </tr>
        <tr>
          <td style="height:50px; border:1px solid #000;"> 4 </td>
          <td style="height:50px; border:1px solid #000;"> 5 </td>
          <td style="height:50px; border:1px solid #000;"> 6 </td>
        </tr>
        <tr>
          <td style="height:50px; border:1px solid #000;"> 7 </td>
          <td style="height:50px; border:1px solid #000;"> 8 </td>
          <td style="height:50px; border:1px solid #000;"> 9 </td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="top-right">
    <table style="font-size:20px;text-align:center;" cellpadding="0" cellspacing="0" border="1px solid black">
        <colgroup>
          <col width="50px" />
          <col width="50px" />
          <col width="50px" />
        </colgroup>
        <tbody>
          <tr>
            <td style="height:50px; border:1px solid #000;font-weight:bold;"> x </td>
            <td style="height:50px; border:1px solid #000;"> 2 </td>
            <td style="height:50px; border:1px solid #000;"> 3 </td>
          </tr>
          <tr>
            <td style="height:50px; border:1px solid #000;font-weight:bold;"> x </td>
            <td style="height:50px; border:1px solid #000;"> 5 </td>
            <td style="height:50px; border:1px solid #000;"> 6 </td>
          </tr>
          <tr>
            <td style="height:50px; border:1px solid #000;font-weight:bold;"> x </td>
            <td style="height:50px; border:1px solid #000;"> 8 </td>
            <td style="height:50px; border:1px solid #000;"> 9 </td>
          </tr>
        </tbody>
      </table>
  </div>
  <div class="clear"></div>
</div>

After some research and debugging I discovered that this behavior was thanks to Python’s **pass by reference** characteristic.  My board had been created using the following function:  

```
[[None * row_size] * row_size]  
```  

This would work perfectly in Ruby or Elixir.  However when building the board using this function in Python, we start off with `None`, which is `Python's null equivalent`. `None` is an object (for more about `None` see the section directly below).  The above line of code creates 3 distinct `None` objects while resolving the code within the inner set of square brackets, thereby producing a single row of distinct elements. It then duplicates that result twice more in order to resolve the code in the outer square brackets, leading to a total of 3 rows, each row having the same references as the previous one. Which is why changing one `None` cell caused a change in 3 cells in the same column.  
&nbsp;  
<span id="none" style="color:#000; font-weight:bold; text-transform:uppercase;">A BRIEF LOOK AT NONE IN PYTHON</span>

- `None` is an object - a class. Not a primitive type, such as int, or True and False.
- In Python **3.x**, the type object was changed from `<type 'NoneType'>` to `<class 'NoneType'>`. However the behaviour of `None` has remained the same from 2.x to 3.x.
- Because `None` is an object, we cannot use it to check if a variable exists. It is a value/object, not an operator used to check a condition.  
&nbsp;  
&nbsp;  
<span id="repl-1" style="color:#000; font-weight:bold;">Toggle lines 1 or 2 in this REPL and run the code to view the Pass by Reference glitch in action</span>   
<iframe height="800px" width="100%" src="https://repl.it/@shibani77/KnottyGentleVisitor?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
&nbsp;  
&nbsp;  
<hr />  
&nbsp;  
<span id="python-scope" style="color:#900; font-weight:bold; text-transform:uppercase;">PYTHON SCOPE</span>  

To discuss **Unbound Local Errors**, I thought it might be helpful to touch upon Python scoping first. Scoping in Python follows the **LEGB Rule** - **Local -> Enclosed -> Global -> Built-in.** Here we work our way from the inside out, from `local` to `built-in`.  

![python-scope]({{ "assets/images/legb.png"| absolute_url }}){: .right-image }

* **Local** scope refers to a variable that is defined within a function body. A variable is always checked for in local scope first.  
In the repl below, if we toggle the lines of code so that line 8 is off, we'll see that our `outer` function executes and prints the local version of variable `my_var`, returning a value of `my inner/local variable`.  
* **Enclosed** scope is created when a function wraps another function, here the outer function is the enclosing scope.  
Back in our repl, we can now `comment line 7` and `uncomment line 8`. On `line 8`, the **nonlocal keyword** will now point Python to using the variable initialized on `line 5, in the outer enclosing scope`. Thus we will see `my enclosed variable` printed to the console.   
* **Global** - as we can imagine, variables assigned at the top-level of a module file are global in scope. `line 13` of the repl prints `my_var from line 2` as it is unable to access the various other instantiations of `my_var` that are all within `enclosed` or `local` scopes. Similar to the `nonlocal` keyword, we can use the **global** keyword to initialize a variable to the global scope from an enclosed or local context.  
* **Built-ins** - no surprises here either, `built-ins` refer to names preassigned in Python's built-in names module, such as `Math, open, range, and SyntaxError`.
<br />
<br />
<br />
<span id="repl-2" style="color:#000; font-weight:bold;">Toggle lines 2, 7 & 8 to see how Python's LEGB scoping works</span>
<iframe height="800px" width="100%" src="https://repl.it/@shibani77/LimeVainHypertalk?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
&nbsp;  
&nbsp;  
<hr />  
&nbsp;  
<span id="unbound-local-variable" style="color:#900; font-weight:bold; text-transform:uppercase;">UNBOUND LOCAL ERROR</span>  
  
Let's say we define a variable `my_var` in the global scope and set it to 5. If we then create a function and attempt to assign a new value to the same variable, but within the function's local context, we'll get an `UnboundLocalError` returned to us.  

The above error occurs because, when we make an `assignment` to a variable in scope, that variable becomes local to that scope and `shadows` any similarly named variable in the outer scope. In the context of the repl below, on `line 7` we are in effect `initializing a new variable my_var to a value of += 1`, and an error is generated. This error is particularly common when working with lists as show in the last example in the repl, and one way to solve accessing a variable declared in the global context is to refer to it as `global` as seen in the method on `line 10` of the repl.
&nbsp;  
&nbsp;  
<span id="repl-3" style="color:#000; font-weight:bold;">Toggle lines 15 - 17 in this REPL to generate Unbound Local Errors</span>  
<iframe height="800px" width="100%" src="https://repl.it/@shibani77/PaltryLiquidLinks?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe> 
&nbsp;  
&nbsp;  
<hr /> 
&nbsp;  
<span id="mutable-default-args" style="color:#900; font-weight:bold; text-transform:uppercase;">MUTABLE DEFAULT ARGUMENTS</span>  

My second bug, this time a gotcha showed up in my implementation of the `Minimax algorithm`.  Syntax that had served me well in the past 2 iterations in Ruby and Elixir was generating strange results.  

<div class="second-example">
  <div class="bottom-left">
    <table style="font-size:20px;text-align:center;" cellpadding="0" cellspacing="0" border="1px solid black">
      <colgroup>
        <col width="50px" />
        <col width="50px" />
        <col width="50px" />
      </colgroup>
      <tbody>
        <tr>
          <td style="height:50px; border:1px solid #000;"> x </td>
          <td style="height:50px; border:1px solid #000;"> x </td>
          <td style="height:50px; border:1px solid #000;"> o </td>
        </tr>
        <tr>
          <td style="height:50px; border:1px solid #000;"> o </td>
          <td style="height:50px; border:1px solid #000;font-weight:bold;"> 5 </td>
          <td style="height:50px; border:1px solid #000;"> o </td>
        </tr>
        <tr>
          <td style="height:50px; border:1px solid #000;"> x </td>
          <td style="height:50px; border:1px solid #000;"> o </td>
          <td style="height:50px; border:1px solid #000;"> x </td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="bottom-middle">
    <table style="font-size:20px;text-align:center;" cellpadding="0" cellspacing="0" border="1px solid black">
      <colgroup>
        <col width="50px" />
        <col width="50px" />
        <col width="50px" />
      </colgroup>
      <tbody>
        <tr>
          <td style="height:50px; border:1px solid #000;"> x </td>
          <td style="height:50px; border:1px solid #000;"> o </td>
          <td style="height:50px; border:1px solid #000;"> x </td>
        </tr>
        <tr>
          <td style="height:50px; border:1px solid #000;"> x </td>
          <td style="height:50px; border:1px solid #000;"> o </td>
          <td style="height:50px; border:1px solid #000;"> x </td>
        </tr>
        <tr>
          <td style="height:50px; border:1px solid #000;"> o </td>
          <td style="height:50px; border:1px solid #000;font-weight:bold;"> 8 </td>
          <td style="height:50px; border:1px solid #000;font-weight:bold;"> 9 </td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="bottom-right">
    <table style="font-size:20px;text-align:center;" cellpadding="0" cellspacing="0" border="1px solid black">
        <colgroup>
          <col width="50px" />
          <col width="50px" />
          <col width="50px" />
        </colgroup>
        <tbody>
          <tr>
            <td style="height:50px; border:1px solid #000;"> x </td>
            <td style="height:50px; border:1px solid #000;"> o </td>
            <td style="height:50px; border:1px solid #000;"> x </td>
          </tr>
          <tr>
            <td style="height:50px; border:1px solid #000;font-weight:bold;"> 4 </td>
            <td style="height:50px; border:1px solid #000;"> o </td>
            <td style="height:50px; border:1px solid #000;font-weight:bold;"> 6 </td>
          </tr>
          <tr>
            <td style="height:50px; border:1px solid #000;font-weight:bold;"> 7 </td>
            <td style="height:50px; border:1px solid #000;font-weight:bold;"> 8 </td>
            <td style="height:50px; border:1px solid #000;font-weight:bold;"> 9 </td>
          </tr>
        </tbody>
      </table>
  </div>
  <div class="clear"></div>
</div>

My first test was set up to check that `Minimax` choses the only open spot, in this case a 5.  

After that, the next test checked if when given a choice between 2 open spots, `Minimax` selected the winning move as opposed to a blocking move. In this case the open spots were 8 and 9, counting from a 1-indexed array.  

A third test checked whether when given a choice of multiple open spots, `Minimax` still chose the winning move. Here the open spots in the 1-indexed array included 4, 6, 7, 8, 9.  

Strangely enough, 5 kept showing up as the selected move in all 3 of these tests, even though 5 was only a valid option in my very first test, and not a valid option in the other 2 mentioned.  It looked like the open spot from my first test was somehow persisting into my next 2 tests.  

Here my mentor alerted me to read up on `Python Gotchas` of which `Mutable Default Arguments`<sup>1</sup> was one.

According to the article,   

>A new list is created once when the function is defined, and the same list is used in each successive call.  

>Python’s default arguments are evaluated once when the function is defined, not each time the function is called (like it is in say, Ruby). This means that if you use a mutable default argument and mutate it, you will and have mutated that object for all future calls to the function as well.  
  
&nbsp;  
Here was the offending line itself:   
```  
def minimax(self, game, depth=0, scores_map={}):  
```

The same goes for any default arguments in a function call that are set to mutable data types in Python. A quick search for mutable types in Python tells us:  

<em>Objects of built-in types like int, float, bool, str, tuple, and unicode are immutable. Objects of built-in types like list, set, and dict are mutable. Custom classes are generally mutable.</em>  

A table to illustrate might be simpler, <a href="#mutable-immutable-types">**see below**</a>.<sup>2</sup>  

My scores map was being mutated with a 5 the first time it was called and then it held on to the 5 for all future calls to that function as well.  

To avoid this common gotcha, the trick was to adjust my code to initialize the scores map variable to `None` instead of an empty dictionary in the function call, and then to check if `scores_map` needed to be initialized later within the body of the function:   

```  
def minimax(self, game, depth=0, scores_map=None):  
  if not scores_map:  
    scores_map = {}   
```  
&nbsp;  
&nbsp; 
<span id="repl-4" style="color:#000; font-weight:bold;">Run the code in the REPL to view this quirk</span>    
<iframe height="800px" width="100%" src="https://repl.it/@shibani77/FlamboyantQuirkyDebugging?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
&nbsp; 
&nbsp;  

<hr />
<div id="mutable-immutable-types" class="mutable-immutable-types">
  &nbsp;
  <span style="color:#900; font-weight:bold; text-transform:uppercase;">MUTABLE & IMMUTABLE TYPES IN PYTHON</span> 
  <table style="font-size:16px;text-align:left;color:#666;margin-top:20px;" cellpadding="10px" cellspacing="0" border="1px solid #666">
      <colgroup>
        <col width="150px" />
        <col width="600px" />
        <col width="150px" />
        <col width="150px" />
      </colgroup>
      <thead>
        <tr class="header">
        <th>Class</th>
        <th>Description</th>
        <th style="text-align:center;">Immutable</th>
        <th style="text-align:center;">Mutable</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td style="height:20px; border:1px solid #666;">bool</td>
          <td style="height:20px; border:1px solid #666;">Boolean value</td>
          <td style="height:20px; border:1px solid #666;text-align:center;">x</td>
          <td style="height:20px; border:1px solid #666;text-align:center;"></td>
        </tr>
        <tr>
          <td style="height:20px; border:1px solid #666;">int</td>
          <td style="height:20px; border:1px solid #666;">Integer (arbitrary magnitude)</td>
          <td style="height:20px; border:1px solid #666;text-align:center;">x</td>
          <td style="height:20px; border:1px solid #666;text-align:center;"></td>
        </tr>
        <tr>
          <td style="height:20px; border:1px solid #666;">float</td>
          <td style="height:20px; border:1px solid #666;">floating-point number</td>
          <td style="height:20px; border:1px solid #666;text-align:center;">x</td>
          <td style="height:20px; border:1px solid #666;text-align:center;"></td>
        </tr>
        <tr>
          <td style="height:20px; border:1px solid #666;">list</td>
          <td style="height:20px; border:1px solid #666;">Mutable sequence of objects</td>
          <td style="height:20px; border:1px solid #666;text-align:center;"></td>
          <td style="height:20px; border:1px solid #666;text-align:center;">x</td>
        </tr>
        <tr>
          <td style="height:20px; border:1px solid #666;">tuple</td>
          <td style="height:20px; border:1px solid #666;">Immutable sequence of objects</td>
          <td style="height:20px; border:1px solid #666;text-align:center;">x</td>
          <td style="height:20px; border:1px solid #666;text-align:center;"></td>
        </tr>
        <tr>
          <td style="height:20px; border:1px solid #666;">str</td>
          <td style="height:20px; border:1px solid #666;">Character string</td>
          <td style="height:20px; border:1px solid #666;text-align:center;">x</td>
          <td style="height:20px; border:1px solid #666;text-align:center;"></td>
        </tr>
        <tr>
          <td style="height:20px; border:1px solid #666;">set</td>
          <td style="height:20px; border:1px solid #666;">Unordered set of distinct objects</td>
          <td style="height:20px; border:1px solid #666;text-align:center;"></td>
          <td style="height:20px; border:1px solid #666;text-align:center;">x</td>
        </tr>
        <tr>
          <td style="height:20px; border:1px solid #666;">frozenset</td>
          <td style="height:20px; border:1px solid #666;">Immutable form of set class</td>
          <td style="height:20px; border:1px solid #666;text-align:center;">x</td>
          <td style="height:20px; border:1px solid #666;text-align:center;"></td>
        </tr>
        <tr>
          <td style="height:20px; border:1px solid #666;">dict</td>
          <td style="height:20px; border:1px solid #666;">Dictionary/Associative mapping</td>
          <td style="height:20px; border:1px solid #666;text-align:center;"></td>
          <td style="height:20px; border:1px solid #666;text-align:center;">x</td>
        </tr>
      </tbody>
    </table>
</div>

<div style="font-size:12px;">
1. Reitz, K [online]. Available at: <a target="blank" href="https://docs.python-guide.org/writing/gotchas/">https://docs.python-guide.org/writing/gotchas/</a> [Accessed 15 Oct. 2018]<br />  
2. Mohan, M [online] Available at: <a target="blank" href="https://medium.com/@meghamohan/mutable-and-immutable-side-of-python-c2145cf72747">https://medium.com/@meghamohan/mutable-and-immutable-side-of-python-c2145cf72747</a><br /> [Accessed 15 Oct. 2018]  
</div>

