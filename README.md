slimerjs
=========

[![npm version](https://badge.fury.io/js/slimerjs.svg)](https://www.npmjs.com/package/slimerjs)

An NPM wrapper for [SlimerJS](https://slimerjs.org/), a scriptable browser for web development and testing.

SlimerJS runs on Gecko, the browser engine behind [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/new/), and aims to be a simple, drop-in replacement for [phantomjs](https://github.com/Medium/phantomjs). Because SlimerJS uses the version of firefox passed in the `SLIMERJSLAUNCHER` environment variable, edge builds of firefox can be used. This can be useful for viewing and testing modern web functionality which is [not yet available in phantomjs](https://github.com/ariya/phantomjs/issues/10031).


Building and Installing
-----------------------

```shell
npm install slimerjs
```

Or grab the source and

```shell
node ./install.js
```

What this is really doing is just grabbing a particular "blessed" (by
this module) version of Slimer. As new versions of Slimer are released
and vetted, this module will be updated accordingly.

The package has been set up to fetch and run Slimer for MacOS (darwin),
Linux based platforms (as identified by nodejs), and -- as of version 0.2.0 --
Windows (thanks to [Domenic Denicola](https://github.com/domenic)).  If you
spot any platform weirdnesses, let us know or send a patch.

Running
-------

```shell
bin/slimerjs [slimer arguments]
```

And npm will install a link to the binary in `node_modules/.bin` as
it is wont to do.

Running via node
----------------

The package exports a `path` string that contains the path to the
slimerjs binary/executable.

Below is an example of using this package via node.

```javascript
var path = require('path')
var childProcess = require('child_process')
var slimerjs = require('slimerjs')
var binPath = slimerjs.path

var childArgs = [
  path.join(__dirname, 'slimerjs-script.js'),
  'some other argument (passed to slimerjs script)'
]

childProcess.execFile(binPath, childArgs, function(err, stdout, stderr) {
  // handle results
})

```

Versioning
----------

The NPM package version tracks the version of SlimerJS that will be installed,
with an additional build number that is used for revisions to the installer.

As such `0.9.1-1` and `0.9.1-2` will both install SlimerJs 0.9.1 but the latter
has newer changes to the installer.

Deciding Where To Get SlimerJS
-------------------------------

By default, this package will download slimerjs from `https://download.slimerjs.org/releases/`.
This should work fine for most people.

##### Downloading from a custom URL

If this site is down, or the Great Firewall is blocking it, you may need to use
a download mirror. To set a mirror, set npm config property `slimerjs_cdnurl`.
Default is ``.

```shell
npm install slimerjs --slimerjs_cdnurl=https://cnpmjs.org/downloads
```

Or add property into your `.npmrc` file (https://www.npmjs.org/doc/files/npmrc.html)

```
slimerjs_cdnurl=https://cnpmjs.org/downloads
```

Another option is to use PATH variable `SLIMERJS_CDNURL`.
```shell
SLIMERJS_CDNURL=https://cnpmjs.org/downloads npm install phantomjs
```

##### Using SlimerJS from disk

If you plan to install slimerjs many times on a single machine, you can
install the `slimerjs` binary on PATH. The installer will automatically detect
and use that for non-global installs.


A Note on SlimerJS
-------------------

SlimerJS is not a library for NodeJS.  It's a separate environment and code
written for node is unlikely to be compatible.  In particular SlimerJS does
not expose a Common JS package loader.

This is an _NPM wrapper_ and can be used to conveniently make Slimer available
It is not a Node JS wrapper.

Standalone SlimerJS scripts can be driven from within a node program by spawning
SlimerJS in a child process.

Read the SlimerJS FAQ for more details: https://slimerjs.org/faq.html

Troubleshooting
---------------

##### Installation fails with `spawn ENOENT`

This is NPM's way of telling you that it was not able to start a process. It usually means:

- `node` is not on your PATH, or otherwise not correctly installed.
- `tar` is not on your PATH. This package expects `tar` on your PATH on Linux-based platforms.

Check your specific error message for more information.

##### Installation fails with `Error: EPERM` or `operation not permitted` or `permission denied`

This error means that NPM was not able to install phantomjs to the file system. There are three
major reasons why this could happen:

- You don't have write access to the installation directory.
- The permissions in the NPM cache got messed up, and you need to run `npm cache clean` to fix them.
- You have over-zealous anti-virus software installed, and it's blocking file system writes.

##### Installation fails with `Error: read ECONNRESET` or `Error: connect ETIMEDOUT`

This error means that something went wrong with your internet connection, and the installer
was not able to download the SlimerJS binary for your platform. Please try again.

##### I tried again, but I get `ECONNRESET` or `ETIMEDOUT` consistently.

Do you live in China, or a country with an authoritarian government? We've seen problems where
the GFW or local ISP blocks https://slimerjs.org, preventing the installer from downloading the
binary.

Try visiting the [the download page](https://download.slimerjs.org/releases) manually.
If that page is blocked, you can try using a different CDN with the `SLIMERJS_CDNURL`
env variable described above.

##### I am behind a corporate proxy that uses self-signed SSL certificates to intercept encrypted traffic.

You can tell NPM and the SlimerJS installer to skip validation of ssl keys with NPM's
[strict-ssl](https://www.npmjs.org/doc/misc/npm-config.html#strict-ssl) setting:

```
npm set strict-ssl false
```

WARNING: Turning off `strict-ssl` leaves you vulnerable to attackers reading
your encrypted traffic, so run this at your own risk!

##### I tried everything, but my network is b0rked. What do I do?

If you install SlimerJS manually, and put it on PATH, the installer will try to
use the manually-installed binaries.

##### I'm on Debian or Ubuntu, and the installer failed because it couldn't find `node`

Some Linux distros tried to rename `node` to `nodejs` due to a package
conflict. This is a non-portable change, and we do not try to support this. The
[official documentation](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager#ubuntu-mint-elementary-os)
recommends that you run `apt-get install nodejs-legacy` to symlink `node` to `nodejs`
on those platforms, or many NodeJS programs won't work properly.

Contributing
------------

Questions, comments, bug reports, and pull requests are all welcome.  Submit them at
[the project on GitHub](https://github.com/graingert/slimerjs/).

Bug reports that include steps-to-reproduce (including code) are the
best. Even better, make them in the form of pull requests.

Author
------

[Dan Pupius](https://github.com/dpup)
([personal website](http://pupius.co.uk)), supported by
[The Obvious Corporation](https://obvious.com/).

License
-------

Copyright 2012 [The Obvious Corporation](https://obvious.com/).

Licensed under the Apache License, Version 2.0.
See the top-level file `LICENSE.txt` and
(https://www.apache.org/licenses/LICENSE-2.0).
