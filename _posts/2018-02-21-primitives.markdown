---
layout: post
title:  "Primitives in Java"
date:   2018-02-21 15:23:55 -0500
categories: java
---
My first week at 8th Light has included working on Java Koans and today I’ve been digging away into Primitive types. It’s always useful to know the Primitive types of any language, and Java is no exception.  

In Java, I was surprised to revisit these and note that there are 8 primitive data types, 2 of which are boolean and char, the rest being numeric. The finer points of these primitive types are notable as well.

Numeric primitive types in Java include integer, long, short, double, float and byte. As is shown in the table below, these numeric primitive types can be signed or unsigned and range in size as well.  The table illustrates the minimum and maximum values of each type and the number of bits each type contains.  

Each of these primitive types can be cast to its Object type, as in a primitive of type integer can be cast into an Integer object via a process called 'Autoboxing'. Reversing out this process is called 'Unboxing'.

The Integer class has properties like MIN_VALUE, MAX_VALUE and SIZE which can tell us more about the kind of values each of these primitive types can hold.

![Numeric Primitives in Java]({{ "/primitive_types.png"| absolute_url }})

More about Java primitives can be found [here] and [also here].

[here]: http://cs.fit.edu/~ryan/java/language/java-data.html
[also here]: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html

<span style="font-size:12px;">Stansifer, R [online] Available at: <http://cs.fit.edu/~ryan/java/language/java-data.html>  
[Accessed 22 Feb. 2018].</span>
