---
layout: post
title:  "Some Common Gotchas in Python"
date:   2018-10-15 15:23:55 -0500
categories: docker
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
</style>

<p style="color:#900; font-weight:bold; text-transform:uppercase;">Some Gotchas I uncovered while building Tictactoe in Python</p>  

My most recent project was writing Tictactoe in Python, a task I thought I had a reasonably clear idea of, after having written Tictactoe in both Ruby and Elixir.  I was thus very surprised to uncover hurdles that seemed to have absolutely no clear solution.  
&nbsp;  
&nbsp;  
___   

&nbsp;   
<p style="color:#900; font-weight:bold; text-transform:uppercase;">PASSING BY REFERENCE</p>  

I encountered the first of these problems when I tried to play the game loop. My game was able to ask the user for input, set the size of the board, create a board, and accurately create a human and computer player. However when I tried to place the first move, my move was not marked in the one square I had picked, but in 3 squares.

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

After some research and debugging I discovered that this behavior was thanks to Python’s **pass by reference** characteristic.  My board had been created using this function:  

```
[[None * row_size] * row_size]  
```  

This would work perfectly in Ruby or Elixir.  However when building the board using this function in Python, we start off with `None`, which is `Python's null equivalent, and an object` (for more about None see the next section).  However while the above line of code creates 3 distinct `None` objects while resolving the code within the inner set of square brackets, thereby producing a single row, it then duplicates that result 3 times in order to resolve the code in the outer square brackets, leading to 3 rows, each row having the same references as the previous one. Which is why changing one `None` changed all 3 in the same column.  

<p style="color:#000; font-weight:bold; text-transform:uppercase;">NONE IN PYTHON</p>

- `None` is an object - a class. Not a basic type, such as digits, or True and False.
- In Python 3.x, the type object was changed to new style classes. However the behaviour of `None` is the same.
- Because `None` is an object, we cannot use it to check if a variable exists. It is a value/object, not an operator used to check a condition.  
  
&nbsp;  
___   

&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">MUTABLE DEFAULT ARGUMENTS</p>  

My second gotcha showed up in my implementation of the `Minimax algorithm`.  Syntax that had served me well in the past 2 iterations in Ruby and Elixir was now having strange effects.  

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

Here my mentor alerted me to `Mutable Default Arguments`<sup>1</sup> 

According to the article,   

>A new list is created once when the function is defined, and the same list is used in each successive call.  

>Python’s default arguments are evaluated once when the function is defined, not each time the function is called (like it is in say, Ruby). This means that if you use a mutable default argument and mutate it, you will and have mutated that object for all future calls to the function as well.  
  
&nbsp;  
Here was the offending line itself:   
```  
def minimax(self, game, depth=0, scores_map={}):  
```

The same goes for any default arguments in a function call that are set to mutable data types in Python. A quick search for mutable types in Python tells us:  

Objects of built-in types like int, float, bool, str, tuple, and unicode are immutable. Objects of built-in types like list, set, and dict are mutable. Custom classes are generally mutable.  A table to illustrate might be simpler, see below.<sup>2</sup>


My scores map was being mutated with a 5 the first time it was called and then it held on to the 5 for all future calls to that function as well.  

To avoid this common gotcha, the trick was to adjust my code to initialize the scores map variable to `None` instead of an empty dictionary in the function call, and then to check if `score_map` needed to be initialized later within the body of the function:   

```  
def minimax(self, game, depth=0, scores_map=None):  
  if not scores_map:  
    scores_map = {}   
```  

<div class="mutable-immutable-types">
  <table style="font-size:16px;text-align:left;color:#666;" cellpadding="10px" cellspacing="0" border="1px solid #666">
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

