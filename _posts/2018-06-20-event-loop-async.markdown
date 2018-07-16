---
layout: post
title:  "The JS Event Loop - Asynchronous"
date:   2018-06-20 15:23:55 -0500
categories: js
---
<style type="text/css">
  .code{
    font-family:"Courier New", Courier, monospace;
  }
</style>

<h4 style="color:#d14">Sequence of Events</h4>  

1. We define the <span class="code">sayHi() function</span>.
2. We define the <span class="code">sayBye() function</span>.
3. We define the <span class="code">display() function</span>. It takes in a <span class="code">data</span> parameter.
4. We set a new variable <span class="code">const futureData</span> to hold the value returned from our fetch function<sup>1</sup>.
5. We then define our <span class="code">fetch</span> function to accept a url that will help us fetch some tweets from Twitter, for example.
6. We finally pass our display function into the .then() callback function, allowing us to display our tweets once our fetch function has returned with them.
  * By default the Fetch API uses the GET method, and provides a .then() success callback and a .catch() failure callback handler.
  * <strong>'Fetch'</strong> will trigger the <span class="code">XHR request</span> and create a promise object.
  * <strong>'Then'</strong> will store the display callback in the created promise object.
  * multiple .then() functions can be chained.  
7. The  <span class="code">setTimeout function</span> is sent to the <span class="code">GLOBAL EXECUTION CONTEXT</span> and it executes.
8. setTimeOut's callback <span class="code">sayHi</span> is sent from the <span class="code">GLOBAL EXECUTION CONTEXT</span> to the <span class="code">web browser APIs</span> .
  * The <span class="code">web browser APIs</span> handle the <span class="code">sayHi function</span> and send it to the <span class="code">CALLBACK QUEUE</span> at 0 milliseconds).
  * <span class="code">setTimeout’s callback function</span>, i.e. <span class="code">sayHi</span> is not allowed back on the <span class="code">CALL STACK</span> until all the code in the GLOBAL EXECUTION CONTEXT executes and the <span class="code">CALL STACK</span> is empty.
9. fetch kicks of an XMLHttpRequest/XHR using the background feature
  * It requires the url and method (get or post).
10. It stores a PROMISE OBJECT in memory with:
  * <strong>a value property</strong> which is empty at first, awaiting the XHR to return with the data, in this case the tweets.
  * <strong>An onFulfilled property</strong> - which will store an array of callback functions that auto-trigger when the value updates and our fetch function succeeds.
  * <strong>An onRejected property</strong> - which will also store an array of callback functions that auto-trigger when the value updates, if our fetch function fails.
  * <span class="code">futureData.onFulfilled.push(display)</span> is what’s happening under the hood but JS does not allow you to do this directly.
11. const futureData stores this PROMISE object which acts like a placeholder.
12. sayBye gets sent to the <span class="code">CALL STACK</span>.
13. <strong><span class="code">sayBye</span> executes, thus appearing first, and we will see 'Thanks for visiting' in the console.</strong>
14. <span class="code">sayBye</span> then gets removed from the <span class="code">CALL STACK</span>.
15. When the tweets are returned from the fetch function, the value property in the promise object is updated with the returned data.  
16. The returned data in the value property is then sent to each of the <span class="code">callback functions</span> in the <span class="code">onFulfilled</span> and <span class="code">onRejected</span> properties.<sup>2</sup>  
17. JS has a <span class="code">MICROTASK</span> or <span class="code">JOB QUEUE</span> which gets executed before the <span class="code">CALLBACK QUEUE</span>.
  * JS gives precedence to the <span class="code">MICROTASK</span> or <span class="code">JOB QUEUE</span>.
  * The <span class="code">MICROTASK</span> or <span class="code">JOB QUEUE</span> gets each callback from the onFulfilled and onRejected properties pushed onto itself with the data in the value property of the <span class="code">PROMISE object</span> as its data argument.
  * <span class="code">display(data)</span> is now queued up in the <span class="code">MICROTASK or <span class="code">JOB QUEUE</span>.
18. Once the <span class="code">CALL STACK</span> and the <span class="code">GLOBAL EXECUTION CONTEXT</span> are empty, the functions in the <span class="code">MICROTASK or <span class="code">JOB QUEUE</span> are sent to the <span class="code">CALL STACK</span>.  
19. <strong>We will now see the Tweets from our fetch function.</strong>
20. It is only after the <span class="code">CALL STACK</span> and the <span class="code">GLOBAL EXECUTION CONTEXT</span> are done with the functions returned from the <span class="code">MICROTASK or <span class="code">JOB QUEUE</span>, that the functions in the <span class="code">CALLBACK QUEUE</span> are pulled onto the <span class="code">CALL STACK</span> and allowed to execute.
21. <strong>It's at this point that we'll see a 'Hello World' appear in the console.</strong>
&nbsp;  
&nbsp;  
<h4 style="color:#d14">Order of Appearance in Console</h4>

1. Thanks for visiting  
2. Tweets from the fetch function  
3. Hello World

![Asynchronous JS Event Loop]({{ "assets/images/js-event-loop-async.png"| absolute_url }})  

<div style="font-size:12px;">
1. <strong>For more on the fetch API, see:</strong><br />
&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API" target="blank">https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API</a><br />
&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://davidwalsh.name/fetch" target="blank">https://davidwalsh.name/fetch</a><br /><br />

2. <strong>States of a promise:</strong><br />
&nbsp;&nbsp;&nbsp;&nbsp;fulfilled - The action relating to the promise succeeded<br />
&nbsp;&nbsp;&nbsp;&nbsp;rejected - The action relating to the promise failed<br />
&nbsp;&nbsp;&nbsp;&nbsp;pending - Hasn't fulfilled or rejected yet<br />
&nbsp;&nbsp;&nbsp;&nbsp;settled (or resolved) - Has been fulfilled or rejected
</div>
