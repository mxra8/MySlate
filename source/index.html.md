---
title: CodeDmx Documentation

toc_footers:
  - <a href='example.html'>Examples</a>
  - <a href='https://github.com/mxra8/slate' target="_blank">Made with Slate</a>

includes:
    - error
    - config
    - func
    - loader
    - router
    - helper_agent
    - helper_array
    - helper_cookie
    - helper_directory
    - helper_download
    - helper_html
    - helper_security
    - helper_string
    - helper_text
    - helper_url
    - class_bcrypt
    - class_cache
    - class_ftp
    - class_image
    - class_mail
    - class_paginator
    - class_querybuilder
    - class_session
    - class_timeago
    - class_upload
    - class_validator
    - class_zip

search: true
---

# Welcome

CodeDmx is a Our PHP framework project that want to enable you to develop projects much faster than you could if you were writing code from scratch, by providing a rich set of libraries for commonly needed task.

CodeDmx PHP Framwork can help you if:

* You want a framework with a small footprint.
* You want a framework that requires nearly zero configuration.
* You want a framework that does not require you to use the command line.
* You want a framework that does not require you to adhere to restrictive coding rules.
* You are not interested in large-scale monolithic libraries like PEAR.
* You do not want to be forced to learn a templating language (although a template parser is optionally available if you desire one).
* You need clear, thorough documentation.

## Server Requirements

1. Web server: Ngnix or Apache (with mod_rewrite)
2. PHP version 7.0 or newer is required.
`It should works on 5.6, but we work with the last version of PHP.`
3. MySQLi PHP Extension
4. OpenSSL PHP Extension
5. Mbstring PHP Extension

## License

**The MIT License (MIT)**

Copyright (c) 2014 - 2017, CodeDMx

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

## Installation

CodeDmx is installed really quickly:

1. Unzip the package.
2. Upload the CodeDmx folders and files to your server.

One additional measure to take in production environments is to disable PHP error reporting and any other development-only functionality.  This can be disable in index.php

**That's it!**
