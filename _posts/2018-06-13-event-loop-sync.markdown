---
layout: post
title:  "The JS Event Loop - Synchronous"
date:   2018-06-13 13:23:55 -0500
categories: js
---
<style type="text/css">
  .code{
    font-family:"Courier New", Courier, monospace;
  }
</style>

Sequence of Events
1. We define the <span class="code">function sayHi()</span>
2. We define the <span class="code">function sayBye()
3. We set up a <span class="code">setTimeOut function</span> to call <span class="code">sayHi</span> at a certain time, in this case, 0 milliseconds.
4. The  <span class="code">setTimeout function</span> is sent to the <span class="code">GLOBAL EXECUTION CONTEXT</span> and executes because of the parentheses.
5. Since <span class="code">setTimeOut</span> is a <span class="code">web browser API function</span>, its callback <span class="code">sayHi</span> is sent from the <span class="code">GLOBAL EXECUTION CONTEXT</span> to the <span class="code">web browser APIs</span> 
  * The <span class="code">web browser APIs</span> handle the <span class="code">sayHi function</span> and send it to the <span class="code">CALLBACK QUEUE</span> at 0 milliseconds).
  * <span class="code">setTimeout’s callback function</span>, i.e. <span class="code">sayHi</span> is never allowed back on the <span class="code">CALL STACK</span> until all the code in the thread of execution completes and the <span class="code">CALL STACK</span> is empty.
6. Meanwhile control flow goes back to the <span class="code">GLOBAL EXECUTION CONTEXT</span> and we hit the <span class="code">sayBye() function</span> next.
7. <span class="code">sayBye</span> gets sent to the <span class="code">CALL STACK</span>.
8. The <span class="code">EVENT LOOP</span> is a constantly running process that checks whether the <span class="code">CALL STACK</span> and <span class="code">CALLBACK QUEUE</span> are empty.
9. It will always allow the items in the <span class="code">CALL STACK</span> to execute first.
10. <span class="code">sayBye</span> executes, <strong>thus appearing first</strong>, and we will see a <strong>'Thanks for visiting'</strong> alert in the browser.
11. <span class="code">sayBye</span> then gets removed from the <span class="code">CALL STACK</span>.
12. Only once the <span class="code">CALL STACK</span> and the <span class="code">GLOBAL EXECUTION CONTEXT</span> are empty, are the functions in the <span class="code">CALLBACK QUEUE</span> pulled onto the <span class="code">CALL STACK</span> and allowed to execute.
13. It's at this point that we'll see a <strong>'Hello World'</strong> alert in the browser.


![Synchronous JS Event Loop]({{ "assets/images/js-event-loop-sync.png"| absolute_url }})
