Groome
======

![](http://github.com/cjerdonek/groome/raw/master/images/mustache-groom.png
"grooming a mustache")

[Groome](http://cjerdonek.github.com/groome/) is a dead-simple,
[Mustache](http://mustache.github.com/)-based standard for language-agnostic
directory templates (i.e. templates for directories of files and
subdirectories).  [Molt](https://github.com/cjerdonek/molt) is the initial
reference implementation.

Project templates are the main use case.  You can use Groome templates to--

* reduce the time to start new projects by eliminating repetitive work, and
* share and encourage project structure best practices.

For example, a Groome template can include initial boilerplate for things
like a README, copyright notices, license info, `.gitignore`, logging
configuration, test harness, directory hierarchy, packaging info, etc. --
all while customizing the boilerplate with project-specific information
like the project name, author, year, etc.

We encourage you to share your best-practice project structures with others
as Groome templates.

Comments and suggestions are welcome on the project's GitHub
[issues page](https://github.com/cjerdonek/groome/issues).


What
----

A Groome template is just a directory of files (and subdirectories) containing
the directory structure, an optional directory of Mustache partials, and
an optional directory of lambda shell scripts.

A Groome implementation renders a Groome template by treating both the name
and contents of each file in the directory structure as a Mustache template.
A single [yaml](http://yaml.org/) or [json](http://www.json.org/)
configuration file provides the context used to render each Mustache
template (together with any lambdas).


Easy Example
------------

Here is a "hello world" example.  The Groome template--

    structure/
        {{project}}.sh.mustache
            echo "{{project}}, world"

with context--

    { "project": "hello" }

yields--

    output/
        hello.sh
            echo "hello, world"

This example can be found in the
[tests/example_easy](https://github.com/cjerdonek/groome/tree/master/tests/example_easy)
directory of this repository.


Advanced Example
----------------

Here is an "advanced" example that illustrates all of the following:
a partial, a lambda-valued variable, and a lambda-valued section.

Again, lambdas and partials are both optional.

    structure/
        {{project}}.sh.mustache
            #!/bin/bash
            {{#hash_comment}}
            Project auto-generated at: {{now}}
            {{>copyright}}
            {{/hash_comment}}
            echo "Running {{project}}..."
    partials/
        copyright.mustache
            Copyright (C) {{year}} {{author}}.
    lambdas/
        now.sh
            #!/bin/bash
            # The -n suppresses the trailing newline.
            echo -n $(date)
        hash_comment.sh
            #!/bin/bash
            while read line; do echo "# $line"; done

With context--

    {
        "project": "awesomeness",
        "author": "Mustachioed Maven",
        "year": 2012
    }

the template above yields (for example)--

    output/
        awesomeness.sh
            #!/bin/bash
            # Project auto-generated at: Sun May 27 11:24:21 PDT 2012
            # Copyright (C) 2012 Mustachioed Maven.
            echo "Running awesomeness..."

The [tests/example_hard](https://github.com/cjerdonek/groome/tree/master/tests/example_hard)
directory contains this example.


Rules
-----

The rules for rendering the structure directory of a Groome template are--

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
5.  Each provided lambda script should be added to the initial rendering
    context, using the script's file name without the extension as the lambda's
    context key.
6.  Lambdas used for sections should be treated as unary.
7.  Input and output should be passed to lambdas as UTF-8 encoded strings
    via `stdin` and `stdout`.


Templates
---------

If you make a Groome template for others to download or fork, you can list it
on the Groome project [wiki](https://github.com/cjerdonek/groome/wiki).

To simplify Groome template distribution, use, and discovery, we suggest that
Groome template projects follow these conventions:

* Store Groome templates in repositories with names prefixed by `groome-`
  (for example `groome-python-script` for a project template for a Python
  script).
* For documentation purposes, provide a sample context by providing a
  configuration file named `sample.json` or `sample.yaml`.  The file should
  contain a name-value collection with the sample context as the value of
  the key `context`.  This allows additional metadata to be included in
  the configuration file, without risk of key collisions with context data.
* Rendering the template with the sample context should provide an
  application that is "runnable" out of the box (for testing and
  demonstration purposes).
* Name the subdirectories as follows:
  * `structure/` the template files, etc.
  * `partials/` the template partials, if any.
  * `lambdas/` the lambda scripts, if any.
  * `expected/` the result of rendering the Groome template against the
    sample context file.


Implementations
---------------

Groome implementations can also be listed on the wiki.

[Molt](http://cjerdonek.github.com/molt/), a Python implementation, is under
construction and should be ready shortly.

To assist implementations, this project has a
[`tests` directory](https://github.com/cjerdonek/groome/tree/master/tests).
This directory contains basic test cases to check isolated aspects
of the rules above.  Implementations can include this project's
Git repository as a [submodule](http://help.github.com/submodules/)
to run these tests as part of a test harness.

The test cases in the `tests` directory do not attempt to thoroughly test
the Mustache implementation underlying a Groome implementation.  For that
we recommend the [Mustache spec](https://github.com/mustache/spec).


Author
------

Groome is authored by [Chris Jerdonek](https://github.com/cjerdonek).  Chris is
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
