---
layout: post
title:  "The JS Event Loop - Asynchronous"
date:   2018-06-20 15:23:55 -0500
categories: js
---

![Asynchronous JS Event Loop]({{ "assets/images/js-event-loop-async.png"| absolute_url }})

<p style="color:#900; font-weight:bold; text-transform:uppercase;">Sequence of Events</p>  

1. We define the `sayHi()` function.  

2. We define the `sayBye()` function.  

3. We define the `display()` function. It takes in a `data` parameter.  

4. We set a new variable `const futureData` to hold the value returned from our `fetch` function<sup>1</sup>.  

5. We then define our `fetch` function to accept a url that will help us fetch some tweets from Twitter, for example.  

6. We finally pass our `display` function into the `.then() callback` function, allowing us to display our tweets once our `fetch` function has returned with them.
  * By default the `Fetch API` uses the `GET` method, and provides a `.then() success callback` and a `.catch() failure callback`.
  * <strong>'Fetch'</strong> will trigger the `XHR request` and create a `promise` object.
  * <strong>'Then'</strong> will store the `display callback` in the created `promise` object.
  * multiple `.then()` functions can be chained.  
&nbsp;  
7. The  `setTimeout` function is sent to the `GLOBAL EXECUTION CONTEXT` and it executes.  

8. `setTimeOut's` callback `sayHi` is sent from the `GLOBAL EXECUTION CONTEXT` to the `web browser APIs` .  
  * The `web browser APIs` handle the `sayHi` function and send it to the `CALLBACK QUEUE` at 0 milliseconds).
  * `setTimeout’s` callback function, i.e. `sayHi` is not allowed back on the `CALL STACK` until all the code in the `GLOBAL EXECUTION CONTEXT` executes and the `CALL STACK` is empty.  
&nbsp;  
9. `fetch` kicks of an `XMLHttpRequest/XHR` using the background feature
  * It requires the url and method (get or post).  
&nbsp;  
10. It stores a `PROMISE OBJECT` in `global memory` with:
  * <strong>a value property</strong> which is empty at first, awaiting the `XHR` to return with the data, in this case the tweets.
  * <strong>An onFulfilled property</strong> - which will store an array of `callback` functions that auto-trigger when the `value` updates and our `fetch` function <strong>succeeds</strong>.
  * <strong>An onRejected property</strong> - which will also store an array of `callback` functions that auto-trigger when the `value` updates, if our `fetch` function <strong>fails</strong>.
  * `futureData.onFulfilled.push(display)` is what’s happening under the hood but JS does not allow you to do this directly.  
&nbsp;  
11. `const futureData` stores this `PROMISE` object which acts like a placeholder.  

12. `sayBye` gets sent to the `CALL STACK`.  

13. `sayBye` <strong>executes, thus appearing first, and we will see 'Thanks for visiting' in the console.</strong>  

14. `sayBye` then gets removed from the `CALL STACK`.  

15. When the tweets are returned from the `fetch` function, the `value` property in the `promise` object is updated with the returned `data`.  

16. This triggers the returned `data` in the `value` property to then be sent to each of the `callback` functions in the `onFulfilled` and `onRejected` properties<sup>2</sup> that are currently being stored in `global memory`.  

17. JS has a `MICROTASK` or `JOB QUEUE` which gets executed before the `CALLBACK QUEUE`.
  * JS gives precedence to the `MICROTASK` or `JOB QUEUE`.
  * The `MICROTASK` or `JOB QUEUE` gets each callback from the `onFulfilled` and `onRejected` properties pushed onto itself with the `data` in the `value` property of the `PROMISE object` as its `data` argument.  
&nbsp;  
18. `display(data)` is now queued up in the `MICROTASK` or `JOB QUEUE`.  

19. Once the `CALL STACK` and the `GLOBAL EXECUTION CONTEXT` are empty, the functions in the `MICROTASK` or `JOB QUEUE` are sent to the `CALL STACK`.  

20. <strong>We will now see the Tweets from our fetch function.</strong>  

21. It is only after the `CALL STACK` and the `GLOBAL EXECUTION CONTEXT` are done with the functions returned from the `MICROTASK` or `JOB QUEUE`, that the functions in the `CALLBACK QUEUE` are pulled onto the `CALL STACK` and allowed to execute.  

22. <strong>It's at this point that we'll see a 'Hello World' appear in the console.</strong>   
&nbsp;  
<p style="color:#900; font-weight:bold; text-transform:uppercase;">Order of Appearance in Console</p>

1. Thanks for visiting  
2. Tweets from the fetch function  
3. Hello World
&nbsp;  
&nbsp;  
<hr />  
&nbsp;  
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
