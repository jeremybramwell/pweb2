\#Web 3.0 Architecture Client
=============================

Table of Contents
-----------------

[Getting Started](#getting-started) First time? Start here.


Quick Notes
-----------
Update all packages:

    npm install

Build this project:

    npm run build

Continuously build (hot module loading):

    npm start

Run tests:

    npm test

The target build directories for this source are:

    ..\..\docroot\public\js
    ..\..\docroot\public\css

Getting Started
---------------

You need to have **[node.js](https://nodejs.org/en/)** installed.  If you have not installed node.js recently, you should [install it/update it](https://nodejs.org/en/download/). 

Not sure?  Check it from the DOS prompt (from any directory):

    node --version

As of this writing (Aug 2016), 4.4.7 is current. Anything 4.3+ is probably good enough at this time. 

Then, check your npm version (npm = node package manager): 

    npm --version

The version of npm installed by node.js sometimes gets behind.  As of this writing make sure your npm version is at least (3.6).  Update it globally with the "-g" flag:

    npm update npm -g

With node.js and npm current, **install the packages** listed in `package.json`.  From the DOS prompt, change directories to the current project directory and run:

    npm install

This will install babel, react, redux, mocha, etc, into your local `node-modules` directory.
