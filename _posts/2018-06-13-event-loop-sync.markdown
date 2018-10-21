---
layout: post
title:  "The JS Event Loop - Synchronous"
date:   2018-06-13 13:23:55 -0500
categories: js
---

![Synchronous JS Event Loop]({{ "assets/images/js-event-loop-sync.png"| absolute_url }})

<p style="color:#900; font-weight:bold; text-transform:uppercase;">Sequence of Events</p>  

1. We define the `sayHi()` function.  

2. We define the `sayBye()` function.  

3. We set up a `setTimeOut` function to call `sayHi` at a certain time, in this case, 0 milliseconds.  

4. The  `setTimeout` function is sent to the `GLOBAL EXECUTION CONTEXT` and executes because of the parentheses.  

5. Since `setTimeOut` is a `web browser API` function, its `callback sayHi` is sent from the `GLOBAL EXECUTION CONTEXT` to the `web browser APIs` .  
  * The `web browser APIs` handle the `sayHi` function and send it to the `CALLBACK QUEUE` at 0 milliseconds).
  * `setTimeout’s callback` function, i.e. `sayHi` is not allowed back on the `CALL STACK` until all the code in the `GLOBAL EXECUTION CONTEXT` completes and the `CALL STACK` is empty.  
&nbsp;  
6. Meanwhile control flow goes back to the `GLOBAL EXECUTION CONTEXT` and we hit the `sayBye()` function next.  

7. `sayBye` gets sent to the `CALL STACK`.  

8. The `EVENT LOOP` is a constantly running process that checks whether the `CALL STACK` and `CALLBACK QUEUE` are empty.  

9. It will always allow the items in the `CALL STACK` to execute first.  

10. <strong>`sayBye` executes, thus appearing first, and we will see 'Thanks for visiting' in the console.</strong>  

11. `sayBye` then gets removed from the `CALL STACK`.  

12. Only once the `CALL STACK` and the `GLOBAL EXECUTION CONTEXT` are empty, are the functions in the `CALLBACK QUEUE` pulled onto the `CALL STACK` and allowed to execute.  

13. <strong>It's at this point that we'll see a 'Hello World' appear in the console.</strong>  
&nbsp;  
&nbsp;  

<p style="color:#900; font-weight:bold; text-transform:uppercase;">Order of Appearance in Console</p>  

1. Thanks for visiting  
2. Hello World
