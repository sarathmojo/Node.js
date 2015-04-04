What is Node.js?
----------------
**Official definition:** Node.js is a platform built on Chrome’s JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

**Simple definition:** A platform that makes writing powerful C/C++ server-side apps, easy by wrapping them in JavaScript.

Node.js under the hood?
-----------------------
![NPM](http://blog.cloudfoundry.org/wp-content/uploads/2012/04/Screen-Shot-2012-04-24-at-5.40.33-PM.png)

Perhaps because of the name Node.js, it might feel like it is built using JavaScript, but it is not. It simply runs JavaScript on the server. It is about 80 percent C/C++ code and about 20 percent JavaScript code. The C/C++ libraries are responsible for running JavaScript (via Google Chrome V8 JS engine) and providing support for HTTP, DNS, and TCP, etc.,–important server-side functionalities. The proportionally smaller JavaScript code mostly consists of libraries or modules to help server-side developers to code.

**Chrome V8 Engine:**
Chrome V8 Engine is Google’s open source, C++ based JavaScript engine that actually runs JavaScript. The Node.js team took this C++ code and added other important libraries like TCP, HTTP and DNS to create Node.js. This is the same engine that is also embedded in the Google Chrome browser that runs JavaScript in the browser as well. This is not yet-another-JavaScript-engine but one that uses innovative techniques like “*Hidden Class Transitions*,” “*JS to Machine Code compilation*” and “*Automatic GC*” to make it one of the fastest JavaScript engines.

**Asynchronous I/O and Event Support (C/C++):**
In order to write a fast and scalable server application, we typically end up writing it in a multi-threaded fashion. While you can build great multi-threaded apps in many languages, it usually requires a lot of expertise to build them correctly. On the other hand, these libraries (along with Chrome’s V8 engine) provide a different architecture that hides the complexities of multi-threaded apps while getting the same or better benefits.

**Multi-threaded HTTP server, Blocking I/O:**
![NPM](http://blog.cloudfoundry.org/wp-content/uploads/2012/04/multiThreadedServer.png)

The above diagram depicts a simplified multi-threaded server. There are four users logging into the multi-threaded server. A couple of the users are hitting refresh buttons causing it to use lot of threads. When a request comes in, one of the threads in the thread pool performs that operation, say, a blocking I/O operation. This triggers the OS to perform context switching and run other threads in the thread pool. And after some time, when the I/O is finished, the OS context switches back to the earlier thread to return the result.

**Architecture Summary:**
Multi-threaded servers supporting a synchronous, blocking I/O model provide a simpler way of performing I/O. But to handle a heavy load, multi-threaded servers end up using more threads because of the direct association to connections. Supporting more threads causes more memory and higher CPU usage due to more context switching among threads.

**Event-driven, Non-Blocking I/O (Node.js server):**
![NPM](http://blog.cloudfoundry.org/wp-content/uploads/2012/04/NodeJS-EventedIOAsyncIO_latest.png)

The above diagram depicts how Node.js server works. At a high level, Node.js server has two parts to it:
At the front, you have Chrome V8 engine (single threaded), event loop and other C/C++ libraries that run your JS code and listen to HTTP/TCP requests.
And at the back of the server, you have libuv (includes libio) and other C/C++ libraries that provide asynchronous I/O.
Whenever a request is made from a browser, mobile device, etc., the main thread running in the V8 engine checks if it is an I/O. if it is an I/O then it immediately delegates that to the backside (kernel level) of the server where one of the threads in the POSIX thread pool actually makes async I/O. Because the main thread is now free, it starts accepting new requests/events.
And at some point when the response comes back from a database or file system, the backend piece generates an event indicating that we have a result from I/O. And when V8 becomes free from what it is currently doing (remember it is single-threaded), it takes the result and returns it to the client.

**Architecture Summary:**
This architecture utilizes an event loop (main thread) at the front and performs asynchronous I/O at the kernel level. By not directly associating connections and threads, this model needs only a main event loop thread and many fewer (kernel) threads to perform I/O. Because there are fewer threads and consequently less context-switching, it uses less memory and also less CPU.

Advantages of Node.js
---------------------
**Savings in I/O cost (i.e., high performance):**
Because of the architecture, Node.js provides high performance like Nginx server as shown below. (As a side note: Nginx uses evented, non-blocking architecture, where as Apache uses multi-threaded architecture. Nginx doesn’t use Node.js, this is just an architecture comparison).
![NPM](http://blog.cloudfoundry.org/wp-content/uploads/2012/04/Screen-Shot-2012-04-24-at-3.31.58-PM.png)

**Savings in Memory:**
Savings in Memory: Again, because of the architecture, Node.js uses relatively very little memory much like Nginx server as shown below.
![NPM](http://blog.cloudfoundry.org/wp-content/uploads/2012/04/Screen-Shot-2012-04-24-at-3.34.07-PM.png)

**JavaScript:**
Node.js uses a familiar and very popular language–JavaScript–and allows engineers to use a single language for both client and server.

**Thousands of libraries:**
High performance and a familiar language is great, but you really need libraries to get started. Although Node.js is relatively new, it already has nearly 11,000 libraries.

Disadvantages of Node.js
------------------------
* Node.js libraries are developed actively with a high rate of change. There are newer versions of libraries literally every month. This can cause version issues and instabilities. Npm shrinkwrap and package.json were introduced a while back to set up standards, but the issue still exists.

* Still many libraries, such as the SAML auth library which is required for enterprise apps, are not available yet.

* The whole callback, event-driven, functional programming aspects of Node.js can add a learning curve burden to server-side programmers of other object-oriented languages. (Note, there are several libraries to help overcome this. One example is async. In addition, developers can also use CoffeeScript which compiles to JavaScript to help with learning curve).

* Asynchronous and event-driven code inherently adds more complexity to the code versus a synchronous code.

* JavaScript has more than its share of “bad parts” and might throw off engineers and newcomers. (Side note: Read some good JavaScript books like: JavaScript: The Good Parts if you are a newcomer.)

When to use Node.js:
--------------------
* Build a (soft) real-time social app like Twitter or a chat app.
* Build high-performance, high I/O, TCP apps like proxy servers, PaaS, databases, etc.
* Build backend logging and processing apps.
* Build great CLI apps similar to vmc-tool, and build tools such as ant or Make.
* Add a RESTful API-based web server in front of an application server.

When not to use Node.js:
------------------------
* Mission-critical (hard) real-time apps like heart monitoring apps or those that are CPU-intensive.
* For simple CRUD apps that don’t have any real-time or high-performance needs, Node.js does not provide much of an advantage over other languages.
* Enterprise apps that might need some specific libraries for which there may not be a Node.js library yet. (However, you could build a polyglot app that uses Java in conjunction to Node.js to help with libraries.)
