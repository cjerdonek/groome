Groom
=====

![](http://github.com/cjerdonek/groom/raw/master/images/groom.png "mustache grooming")

[Groom](http://cjerdonek.github.com/groom/) is a dead-simple,
[Mustache](http://mustache.github.com/)-based standard for language-agnostic
project templates.

A Groom template is just a directory of files (and subdirectories) containing
a project structure, an optional directory of Mustache partials, and
an optional directory of lambda shell scripts.

A Groom implementation creates a new project by treating both the name and
contents of each file in the project directory as a Mustache template.
A single [yaml](http://yaml.org/) or [json](http://www.json.org/)
configuration file provides the context used to render each Mustache
template (together with any lambdas).

We encourage you to share your best-practice project structures with others
as Groom templates.  Comments and suggestions on Groom are welcome on the
project's GitHub [issues page](https://github.com/cjerdonek/groom/issues).


Easy Example
------------

Here is a "hello world" example.  The Groom template--

    project/
        {{project}}.sh.mustache
            echo "{{project}}, world"

with context--

    { "project": "hello" }

yields--

    output/
        hello.sh
            echo "hello, world"

This example can be found in the
[tests/example_easy](https://github.com/cjerdonek/groom/tree/master/tests/example_easy)
directory of this repository.


Advanced Example
----------------

Here is an "advanced" example that contains both partials and lambdas
(again, lambdas and partials are both optional).

    lambdas/
        hash_comment.sh
            #!/bin/bash
            while read line; do echo "# $line"; done
    partials/
        copyright.mustache
            Copyright (C) {{year}} {{author}}.
    project/
        {{project}}.sh.mustache
            #!/bin/bash
            {{#hash_comment}}
            {{>copyright}}
            {{/hash_comment}}
            echo "Running {{project}}..."

With context--

    {
        "project": "awesomeness",
        "author": "Mustachioed Maven",
        "year": 2012
    }

the template above yields--

    output/
        awesomeness.sh
            #!/bin/bash
            # Copyright (C) 2012 Mustachioed Maven.
            echo "Running awesomeness..."

The [tests/example_hard](https://github.com/cjerdonek/groom/tree/master/tests/example_hard)
directory contains this example.


Rules
-----

The rules for rendering the project directory of a Groom template are--

1.  All file names and directory names are treated as Mustache templates,
    after any pre-processing of the name described below.
2.  If a file name ends in `.mustache`, the trailing `.mustache` is stripped
    from the file name and the file contents treated as a Mustache template.
3.  To leave the contents of a Mustache file alone, end the file name in
    `.skip.mustache`.  For such files, the trailing `.skip.mustache` is
    changed to `.mustache` and the file contents are left unchanged.  This is
    the only exception to the previous rule.
4.  If a file name does not end in `.mustache`, the contents of the file
    are copied as is and not treated as a Mustache template.
5.  Any lambda scripts provided should be added to the initial rendering
    context, using the file name without the extension as the corresponding
    context key.
6.  All lambdas coming from lambda scripts should be considered unary,
    with `stdin` and `stdout` used for input and output.


Templates
---------

If you make a Groom template for others to download or fork, you can list it
on the Groom project [wiki](https://github.com/cjerdonek/groom/wiki).

To simplify Groom template distribution, use, and discovery, we suggest that
Groom template projects follow these conventions:

* Store groom templates in repositories with names prefixed by `groom-`
  (for example `groom-python2and3-script`).
* Name the project structure directory `project`, the partials directory
  `partials`, and the lambda directory `lambdas`.
* For documentation purposes, provide a sample context by providing a
  configuration file named `sample.json` or `sample.yaml`.  The file should
  contain a name-value collection with the sample context as the value of
  the key `context`.  This allows additional metadata to be included in
  the configuration file, without risk of key collisions with context data.
* Rendering the template with the sample context should provide an
  application that is "runnable" out of the box (for testing and
  demonstration purposes).


Implementations
---------------

Groom implementations can also be listed on the wiki.

[Molt](http://cjerdonek.github.com/molt/), a Python implementation, is under
construction and should be ready shortly.

To assist implementations, this project has a
[`tests` directory](https://github.com/cjerdonek/groom/tree/master/tests).
This directory contains basic test cases to check isolated aspects
of the rules above.  Implementations can include this project's
Git repository as a [submodule](http://help.github.com/submodules/)
to run these tests as part of a test harness.

The test cases in the `tests` directory do not attempt to thoroughly test
the Mustache implementation underlying a Groom implementation.  For that
we recommend the [Mustache spec](https://github.com/mustache/spec).


Author
------

Groom is authored by [Chris Jerdonek](https://github.com/cjerdonek).  Chris is
also the current [Pystache](https://github.com/defunkt/pystache) maintainer.


Copyright
---------

Copyright (C) 2012 Chris Jerdonek.  All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice,
  this list of conditions and the following disclaimer.
* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.
* The names of the copyright holders may not be used to endorse or promote
  products derived from this software without specific prior written
  permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
POSSIBILITY OF SUCH DAMAGE.
