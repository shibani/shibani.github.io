---
layout: post
title:  "Some Common Gotchas in Python"
date:   2018-10-15 15:23:55 -0500
categories: docker
---
<style type="text/css">
  .left-image {
    float:left;
    margin-left:20px;
    width:400px;
  }
  .right-image {
    float:right;
    margin-left:20px;
    width:400px;
  }
</style>

<p style="color:#900; font-weight:bold; text-transform:uppercase;">Some Gotchas I uncovered while building Tictactoe in Python</p>  

My most recent project was writing Tictactoe in Python, a task I thought I had a pretty clear picture of, after having written Tictactoe in both Ruby and Elixir.  I was thus very surprised to uncover hurdles that seemed to have absolutely no clear solution.  
&nbsp;  
&nbsp;  
___   

&nbsp;   
<p style="color:#900; font-weight:bold; text-transform:uppercase;">PASSING BY REFERENCE</p>  

I uncovered one such problem as soon as I was able to play the game loop. My game was able to ask the user for input, set the size of the board, create a board, and accurately create a human and computer player.  

However when I tried to mark a first move, my move was not placed in the one square I had picked, but in the entire column.  
In other words, instead of looking like figure 1 below, my board looked like figure 2!  

After some serious debugging I discovered that this behavior was thanks to Python’s **pass by reference** characteristic.  My board had been created using this function:  

```[None * row_size] * row_size]```  

This would work perfectly in Ruby or Elixir.  However when building the board using this function in Python, we start off with None, which is Python's null equivalent.  However unlike **null**, None is an object, and the above line of code duplicated the reference to None instead of creating a copy of it. Which is why changing one None changed all 3 in the same column.  

<p style="color:#000; font-weight:bold; text-transform:uppercase;">NONE IN PYTHON</p>

- None is an object - a class. Not a basic type, such as digits, or True and False.
- In Python 3.x, the type object was changed to new style classes. However the behaviour of None is the same.
- Because None is an object, we cannot use it to check if a variable exists. It is a value/object, not an operator used to check a condition.
&nbsp;  
&nbsp;  
___   

&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">MUTABLE DEFAULT ARGUMENTS</p>  

My second gotcha showed in my implementation of Minimax algorithm.  Syntax that had served me well in the past 2 iterations in Ruby and Elixir was now having strange effects.  

My first test was set up to check that Minimax choses the only open spot, in this case a 5.  

After that, the next test checked if when given a choice between 2 open spots, Minimax selected the winning move as opposed to a blocking move. In this case the open spots were 8 and 9, counting from a 1-indexed array.  

A third test checked whether when given a choice of multiple open spots, Minimax still chose the winning move. Here the open spots in the 1-indexed array included 4, 6, 7, 8, 9.  

Strangely enough, 5 kept showing up as the selected move in all 3 of these tests, even though 5 was not a valid option in 2 of the 3 tests.  It looked like the open spot from my first test was somehow persisting into my next 2 tests.  

I was about to dig into the depths of scoping in python when my mentor alerted me to <https://docs.python-guide.org/writing/gotchas/> - Mutable Default Arguments  

According to the article,   

```‘A new list is created once when the function is defined, and the same list is used in each successive call.
Python’s default arguments are evaluated once when the function is defined, not each time the function is called (like it is in say, Ruby). This means that if you use a mutable default argument and mutate it, you will and have mutated that object for all future calls to the function as well.’```

Here was the offending line itself:   
```def minimax(self, game, depth=0, scores_map={]):```

The same goes for any default arguments in a function call that are set to mutable data types in Python. A quick search for mutable types in Python tells us:  

Objects of built-in types like (int, float, bool, str, tuple, unicode) are immutable. Objects of built-in types like (list, set, dict) are mutable. Custom classes are generally mutable.  

A table to illustrate might be simpler: <https://medium.com/@meghamohan/mutable-and-immutable-side-of-python-c2145cf72747>

My scores map was being mutated with a 5 the first time it was called and then it held on to the 5 for all future calls to that function as well.  

To avoid this common gotcha, I adjusted my code to initialize the scores map variable to ’None’ instead of an empty dictionary, like so:  

```def minimax(self, game, depth=0, scores_map=None):```

and then later within the body of the function, check if the score_map needed to be initialized:  

```if not scores_map:
	scores_map = {}```


