\#Web 3.0 Architecture Client
=============================

Table of Contents
-----------------

[Getting Started](#getting-started) First time? Start here.

[Adding a new Package](#adding-a-new-package)

[Package Maintenance](#package-maintenance)

[What are all these packages?](#what-are-all-these-packages-)

[Setup for use with #Web](#setup-for-use-with-web)

[About This Readme](#about-this-readme)


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

Adding a new Package
--------------------
When it is determined that a new package is needed install it locally with 

    npm install _packagename_ --save

Or if it is a loader, test, or other development only package:

    npm install _packagename_ --save-dev

The `--save(-dev)` will insert the appropriate line into `package.json`. 

SVN `Commit` the new `package.json`, and let others know that next time they update they should run `npm install` to get the new packages. 

Package Maintenance
-------------------

On a regular basis, node modules should be checked for being up to date.  Run:
* `npm ls` to view the list of packages with their dependencies.  This will also print out unmet dependencies (packages that should be installed) and extraneous dependencies (packages that may be removed).

* `npm outdated` to determine if packages are outdated.  

* `npm update _packagename_` when a package needs to be updated.  When a more up to date package is confirmed as okay, use the `--save` or `--save-dev` flag to update the `package.json`.

* `npm uninstall _packagename_ --save` when a package is no longer needed.  

* `npm prune` to purge extraneous packages.

What are all these packages?
----------------------------

* **[webpack](http://webpack.github.io/docs/)** - the build tool
  * **webpack-dev-server** - a light weight "express" server for use with webpack which implements hot module replacement.
  * **webpack-merge**, **clean-webpack-plugin**, **extract-text-webpack-plugin** - tools used in webpack.config.js
  * **[babel,css,file,json,sass,etc.]-[loader](http://webpack.github.io/docs/using-loaders.html)** - webpack loaders needed at build time


* **[babel](https://babeljs.io/)** - the ES2015, JSX transpiler
  * **babel-preset-**... - plugins that provide babel with language specifics


* **[eslint](http://eslint.org/)** - code quality linter that understands es2015

* **[mocha](http://mochajs.org/)** - a test runner, and testing framework

* **[expect](https://github.com/mjackson/expect/blob/master/README.md)** - a testing assertion and spy library for use with mocha

* **[react](https://facebook.github.io/react/docs/getting-started.html)** - library to implement UI components
  * **react-pure-render** - 
  * **react-immutable-proptypes** - 


* **[redux](http://redux.js.org/index.html)** - helpers to implement our flavor of flux implementation
  * **[react-redux](http://redux.js.org/docs/basics/UsageWithReact.html)** - react-redux connector
  * **[redux-thunk](https://github.com/gaearon/redux-thunk/blob/master/README.md)** - redux middleware - allows actions to return a function rather than an action.
  * **[flux-standard-action](https://github.com/acdlite/flux-standard-action/blob/master/README.md), redux-actions** - standardized actions help make the shape of the action predictable.


* **[immutable](https://facebook.github.io/immutable-js/)** - a library that helps implement immutable structures

Setup for use with #Web
-----------------------

To setup for use with #Web: 

  1. Edit Apache configuration: In the folder, _C:\Program Files (x86)\MEDITECH\Apache\conf_, edit `httpd-vhosts.conf` to add: 
  ```
  ProxyPass /pwebDev http://localhost:3000
  ProxyPassReverse /pwebDev http://localhost:3000
  ```

  In the same folder, edit `httpd.conf` to add:
  ```
  LoadModule proxy_http_module modules/mod_proxy_http.so
  ```

  2. In your w12 installation, under the _w12\System_ folder, make sure you have a copy of `IPL*` and `WPL*` from _w12\WebClient\system_.  
  
  If not copy and paste the entire content of the _WebClient\system_ folder into _w12\System_.

  3. Then, edit `WPLTool.ini` in _w12\System_ to change:
  ```
  VirtualURLs = {{system|[w]\..\WebServer\WebRoot\system}|{pub|[w]\..\WebServer\WebRoot\pub}|{amb|[w]\..\WebServer\WebRoot\amb}}
  ```
  to
  ```
  VirtualURLs = {{pweb|[w]\..\WebClient\src\pweb\w12\build}|{system|[w]\..\Services\WebServer-w11\WebRoot\system}|{pub|[w]\..\Services\WebServer-w11\WebRoot\pub}|{amb|[w]\..\Services\WebServer-w11\WebRoot\amb}}
  ```

  4. Make sure your _w12\System\Environment\StateVariables.txt_ file is present. 

  5. Update your _w12\System\RunTimeInfo\Root Table.fs_ to point to your w12 path.  E.g.: 
  ```
  {Programs|Foc|C:|SVNFocus\Versions\Version13\w12\PgmObject\Foc|C:||NoCache}|
  ```
  And translate it.  

  6. Retranslate everything in w12.  (E.g., run \SVNFocus\Utilities\Magic.exe FolderHandler.mas)

  7. Update then translate test programs that you want to run.  

  E.g., `w12\PgmSource\WebTest\WebTest.Comp.InputText-w11.WR.focus`
  You may need to make some changes.  E.g., _WidthUnits_ is obsolete and should be removed. 

  8. Retranslate the WebTest Launcher: _w12\PgmSource\WebTest\WebTest.Util.Launcher-w11.WR.focus_

  9. Make sure you have a WPL w12 LaunchPoint.  
    - In HeidiSQL, on the _data_ tab of the `wpl/launchpoints` table add a command line like this:
      ```
      "C:\SVNFocus\Versions\Version13\w12\System\magic_console.exe" "C:\SVNFocus\Versions\Version13\w12\PgmObject\Foc\FocXweb.Start.P.mps" /wpl /rtn:/WebTest-WebTest.Util.Launcher-w11.WR.mthr
      ```
      make sure your paths are correct. 

    - on the _data_ tab of the `wpl/launchpointx` table, add the index for the launchpoint you just added.

  10. Restart your **Web application server**.

  11. Start your **Dev server** (`npm start`)

You should now be able to run w12 under WPL.  

To test without WPL or TCP Service use this URL:
    https://myhost.meditech.com/pwebDev/

To test with WPL, use this URL: 
    https://myhost.meditech.com/wpl/

About This Readme
-----------------

If you are reading this markdown file in plain text, hopefully you found it readable.  

However, to read it in the intended format, try the following:  Find the [Markdown Preview Plus](https://chrome.google.com/webstore/detail/markdown-preview-plus/febilkbfcbhebfnokafefeacimjdckgl) extension in the Chrome store.  Install it.  Then enable it by going to `Menu > More tools > Extensions` and checking 

[x] Allow access to file URLs

You can then just drag the `Readme.md` into Chrome. 

There are also plugins available for Firefox and the Atom editor.

If you are editing the `Readme` and want to view your edits live in an on-line editor, there are a few choices.  http://dillinger.io/ is popular, works pretty well, and lets you save the .md format or .html format.