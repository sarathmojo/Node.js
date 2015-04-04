What is Node.js?
----------------
**Official definition:** Node.js is a platform built on Chrome’s JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

**Simple definition:** A platform that makes writing powerful C/C++ server-side apps easy by essentially wrapping them in JavaScript.

Node.js under the hood?
-----------------------
![NPM](http://blog.cloudfoundry.org/wp-content/uploads/2012/04/Screen-Shot-2012-04-24-at-5.40.33-PM.png)
While at a first glance, perhaps because of the name Node.js, it might feel like it is built using JavaScript, but it is not. It simply runs JavaScript on the server. It is about 80 percent C/C++ code and about 20 percent JavaScript code. The C/C++ libraries are responsible for running JavaScript (via Google Chrome V8 JS engine) and providing support for HTTP, DNS, and TCP, etc.,–important server-side functionalities. The proportionally smaller JavaScript code mostly consists of libraries or modules to help make server-side developers’ lives a lot simpler.

Chrome V8 Engine:
-----------------
Chrome V8 Engine is Google’s open source, C++ based JavaScript engine that actually runs JavaScript. The Node.js team took this C++ code and added other important libraries like TCP, HTTP and DNS to create Node.js. This is the same engine that is also embedded in the Google Chrome browser that runs JavaScript in the browser as well. This is not yet-another-JavaScript-engine but one that uses innovative techniques like “Hidden Class Transitions,” “JS to Machine Code compilation” and “Automatic GC” to make it one of the fastest JavaScript engines.

Asynchronous I/O and Evented Support (C/C++):
---------------------------------------------
In order to write a fast and scalable server application, we typically end up writing it in a multi-threaded fashion. While you can build great multi-threaded apps in many languages, it usually requires a lot of expertise to build them correctly. On the other hand, these libraries (along with Chrome’s V8 engine) provide a different architecture that hides the complexities of multi-threaded apps while getting the same or better benefits.

**An example multi-threaded HTTP server using blocking I/O**
![NPM](http://blog.cloudfoundry.org/wp-content/uploads/2012/04/multiThreadedServer.png)
The above diagram depicts a simplified multi-threaded server. There are four users logging into the multi-threaded server. A couple of the users are hitting refresh buttons causing it to use lot of threads. When a request comes in, one of the threads in the thread pool performs that operation, say, a blocking I/O operation. This triggers the OS to perform context switching and run other threads in the thread pool. And after some time, when the I/O is finished, the OS context switches back to the earlier thread to return the result.
**Architecture Summary:**
Multi-threaded servers supporting a synchronous, blocking I/O model provide a simpler way of performing I/O. But to handle a heavy load, multi-threaded servers end up using more threads because of the direct association to connections. Supporting more threads causes more memory and higher CPU usage due to more context switching among threads.



[![NPM](https://nodei.co/npm/config.svg?downloads=true&downloadRank=true)](https://nodei.co/npm/config/)&nbsp;&nbsp;
[![Build Status](https://secure.travis-ci.org/lorenwest/node-config.svg?branch=master)](https://travis-ci.org/lorenwest/node-config)&nbsp;&nbsp;
[release notes](https://github.com/lorenwest/node-config/blob/master/History.md)

Introduction
------------

Node-config organizes hierarchical configurations for your app deployments.

It lets you define a set of default parameters,
and extend them for different deployment environments (development, qa,
staging, production, etc.).

Configurations are stored in [configuration files](https://github.com/lorenwest/node-config/wiki/Configuration-Files) within your application, and can be overridden and extended by [environment variables](https://github.com/lorenwest/node-config/wiki/Environment-Variables),
 [command line parameters](https://github.com/lorenwest/node-config/wiki/Command-Line-Overrides), or [external sources](https://github.com/lorenwest/node-config/wiki/Configuring-from-an-External-Source).

This gives your application a consistent configuration interface shared among a
[growing list of npm modules](https://www.npmjs.org/browse/depended/config) also using node-config.

Project Guidelines
------------------

* *Simple* - Get started fast
* *Powerful* - For multi-node enterprise deployment
* *Flexible* - Supporting multiple config file formats
* *Lightweight* - Small file and memory footprint
* *Predictable* - Well tested foundation for module and app developers

Quick Start
---------------
The following examples are in JSON format, but configurations can be in other [file formats](https://github.com/lorenwest/node-config/wiki/Configuration-Files#file-formats).

**Install in your app directory, and edit the default config file.**

    $ npm install config
    $ mkdir config
    $ vi config/default.json

    {
      // Customer module configs
      "Customer": {
        "dbConfig": {
          "host": "localhost",
          "port": 5984,
          "dbName": "customers"
        },
        "credit": {
          "initialLimit": 100,
          // Set low for development
          "initialDays": 1
        }
      }
    }

**Edit config overrides for production deployment:**

    $ vi config/production.json

    {
      "Customer": {
        "dbConfig": {
          "host": "prod-db-server"
        },
        "credit": {
          "initialDays": 30
        }
      }
    }

**Use configs in your code:**

    var config = require('config');
    ...
    var dbConfig = config.get('Customer.dbConfig');
    db.connect(dbConfig, ...);

    if (config.has('optionalFeature.detail')) {
      var detail = config.get('optionalFeature.detail');
      ...
    }

`config.get()` will throw an exception for undefined keys to help catch typos and missing values.
Use `config.has()` to test if a configuration value is defined.

**Start your app server:**

    $ export NODE_ENV=production
    $ node my-app.js

Running in this configuration, the `port` and `dbName` elements of `dbConfig`
will come from the `default.json` file, and the `host` element will
come from the `production.json` override file.

Articles
--------

* [Configuration Files](https://github.com/lorenwest/node-config/wiki/Configuration-Files)
* [Common Usage](https://github.com/lorenwest/node-config/wiki/Common-Usage)
* [Environment Variables](https://github.com/lorenwest/node-config/wiki/Environment-Variables)
* [Reserved Words](https://github.com/lorenwest/node-config/wiki/Reserved-Words)
* [Command Line Overrides](https://github.com/lorenwest/node-config/wiki/Command-Line-Overrides)
* [Multiple Node Instances](https://github.com/lorenwest/node-config/wiki/Multiple-Node-Instances)
* [Sub-Module Configuration](https://github.com/lorenwest/node-config/wiki/Sub-Module-Configuration)
* [Configuring from a DB / External Source](https://github.com/lorenwest/node-config/wiki/Configuring-from-an-External-Source)
* [External Configuration Management Tools](https://github.com/lorenwest/node-config/wiki/External-Configuration-Management-Tools)
* [Examining Configuration Sources](https://github.com/lorenwest/node-config/wiki/Examining-Configuration-Sources)
* [Using Config Utilities](https://github.com/lorenwest/node-config/wiki/Using-Config-Utilities)
* [Upgrading from Config 0.x](https://github.com/lorenwest/node-config/wiki/Upgrading-From-Config-0.x)

Contributors
------------
<table id="contributors"><tr><td><img src=https://avatars.githubusercontent.com/u/373538?v=3><a href="https://github.com/lorenwest">lorenwest</a></td><td><img src=https://avatars.githubusercontent.com/u/25829?v=3><a href="https://github.com/markstos">markstos</a></td><td><img src=https://avatars.githubusercontent.com/u/791137?v=3><a href="https://github.com/josx">josx</a></td><td><img src=https://avatars.githubusercontent.com/u/133277?v=3><a href="https://github.com/enyo">enyo</a></td><td><img src=https://avatars.githubusercontent.com/u/1077378?v=3><a href="https://github.com/arthanzel">arthanzel</a></td><td><img src=https://avatars.githubusercontent.com/u/1656140?v=3><a href="https://github.com/eheikes">eheikes</a></td></tr><tr><td><img src=https://avatars.githubusercontent.com/u/355800?v=3><a href="https://github.com/diversario">diversario</a></td><td><img src=https://avatars.githubusercontent.com/u/138707?v=3><a href="https://github.com/th507">th507</a></td><td><img src=https://avatars.githubusercontent.com/u/506460?v=3><a href="https://github.com/Osterjour">Osterjour</a></td><td><img src=https://avatars.githubusercontent.com/u/842998?v=3><a href="https://github.com/nsabovic">nsabovic</a></td><td><img src=https://avatars.githubusercontent.com/u/175627?v=3><a href="https://github.com/axelhzf">axelhzf</a></td><td><img src=https://avatars.githubusercontent.com/u/1246875?v=3><a href="https://github.com/jaylynch">jaylynch</a></td></tr><tr><td><img src=https://avatars.githubusercontent.com/u/145742?v=3><a href="https://github.com/jberrisch">jberrisch</a></td><td><img src=https://avatars.githubusercontent.com/u/1918551?v=3><a href="https://github.com/nitzan-shaked">nitzan-shaked</a></td><td><img src=https://avatars.githubusercontent.com/u/3058150?v=3><a href="https://github.com/Alaneor">Alaneor</a></td><td><img src=https://avatars.githubusercontent.com/u/498929?v=3><a href="https://github.com/roncli">roncli</a></td><td><img src=https://avatars.githubusercontent.com/u/1355559?v=3><a href="https://github.com/superoven">superoven</a></td><td><img src=https://avatars.githubusercontent.com/u/4425455?v=3><a href="https://github.com/ncuillery">ncuillery</a></td></tr><tr><td><img src=https://avatars.githubusercontent.com/u/959858?v=3><a href="https://github.com/laktak">laktak</a></td><td><img src=https://avatars.githubusercontent.com/u/57770?v=3><a href="https://github.com/bertrandom">bertrandom</a></td><td><img src=https://avatars.githubusercontent.com/u/157303?v=3><a href="https://github.com/cmcculloh">cmcculloh</a></td><td><img src=https://avatars.githubusercontent.com/u/125062?v=3><a href="https://github.com/keis">keis</a></td><td><img src=https://avatars.githubusercontent.com/u/28898?v=3><a href="https://github.com/DMajrekar">DMajrekar</a></td><td><img src=https://avatars.githubusercontent.com/u/535667?v=3><a href="https://github.com/jasonhansel">jasonhansel</a></td></tr><tr><td><img src=https://avatars.githubusercontent.com/u/2533984?v=3><a href="https://github.com/jonjonsonjr">jonjonsonjr</a></td><td><img src=https://avatars.githubusercontent.com/u/157474?v=3><a href="https://github.com/k-j-kleist">k-j-kleist</a></td><td><img src=https://avatars.githubusercontent.com/u/12112?v=3><a href="https://github.com/GUI">GUI</a></td><td><img src=https://avatars.githubusercontent.com/u/16861?v=3><a href="https://github.com/abh">abh</a></td><td><img src=https://avatars.githubusercontent.com/u/7698940?v=3><a href="https://github.com/robludwig">robludwig</a></td><td><img src=https://avatars.githubusercontent.com/u/811927?v=3><a href="https://github.com/bolgovr">bolgovr</a></td></tr></table>

License
-------

May be freely distributed under the [MIT license](https://raw.githubusercontent.com/lorenwest/node-config/master/LICENSE).

Copyright (c) 2010-2015 Loren West 
[and other contributors](https://github.com/lorenwest/node-config/graphs/contributors)

