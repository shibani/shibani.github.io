---
layout: post
title:  "Size, Length and Count in Ruby"
date:   2018-05-29 15:23:55 -0500
categories: ruby
---
The Differences between Size, Length and Count

Size, length and count are three different methods to find the length of an array in Ruby.  I thought it would be helpful to list the subtle differences among each.

**SIZE** is not part of the Enumerable class but is a method on concrete classes such as String or Array. This makes it a lot faster than an Enumerable method, it usually runs in O(1) or constant time.  

<https://ruby-doc.org/core-2.5.1/Enumerable.html>  
<https://ruby-doc.org/core-2.2.0/Array.html>  
<https://ruby-doc.org/core-2.2.0/String.html>   

**LENGTH** is an alias for Size.

**COUNT** is part of the Enumerable class.  It can be used with a block or argument and is therefore much slower.  It can also be used without a block or args if necessary, just returning the count of the array it is called on.  
From the Ruby docs: If a block is given, counts the number of elements for which the block returns a true value.

<span style="font-size:12px;"> <http://batsov.com/articles/2014/02/17/the-elements-of-style-in-ruby-number-13-length-vs-size-vs-count/>  
[Accessed 29 May 2018].
