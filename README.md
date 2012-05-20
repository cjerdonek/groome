Groom
=====

![](http://github.com/cjerdonek/groom/raw/master/images/groom.png "mustache grooming")

[Groom](http://cjerdonek.github.com/groom/) is a dead-simple,
[Mustache](http://mustache.github.com/)-based standard for project templates.

A Groom template is just a directory of files (and subdirectories) containing
a project structure, and an optional directory of Mustache partials.

A Groom implementation creates a new project by treating both the name and
contents of each file in the template directory as a Mustache template.
A single [yaml](http://yaml.org/) or [json](http://www.json.org/) configuration
file provides the context used to render every Mustache template.


Example
-------

For example, the following Groom template (in the directory
[tests/demo](https://github.com/cjerdonek/groom/tree/master/tests/demo))--

	partials/
	    copyright.mustache
			# Copyright (C) {{year}} {{author}}.
	project/
		{{project}}.py.mustache
		    {{>copyright}}
			print "Running {{project}}..."

with context--

	{
        "project": "awesomeness",
        "author": "Mustachioed Maven",
        "year": 2012
	}

yields--

	output/
		awesomeness.py
			# Copyright (C) 2012 Mustachioed Maven.
			print "Running awesomeness..."

Rules
-----

The rules for rendering the contents of a Groom template directory are--

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


Templates
---------

If you make a Groom template for others to use, you can list it on the Groom
project [wiki](https://github.com/cjerdonek/groom/wiki).

To simplify Groom template distribution, use, and discovery, we suggest that
Groom template projects follow these conventions:

* Store groom templates in repositories with names prefixed by `groom-`
  (for example `groom-python2and3-script`).
* Name the project structure directory `project` and the partials
  directory `partials`.
* Provide an example configuration file named `sample.json` or `sample.yaml`
  that contains a name-value collection.
* The sample context should be the value of the key `context` in the
  configuration file.  This allows for the inclusion of additional metadata
  in the configuration file, without risk of key collisions with context data.


Implementations
---------------

Groom implementations can also be listed on the wiki.

[Molt](https://github.com/cjerdonek/molt), a Python implementation, is under
construction and should be ready shortly.

To assist implementations, the project has
[a `tests` directory](https://github.com/cjerdonek/groom/tree/master/tests).
The directory contains a number of test cases to check various isolated
aspects of the rules above.  Implementations can include this project's
Git repository as a [submodule](http://help.github.com/submodules/)
to run these tests more easily as part of an implementation's test harness.

The test cases in the `tests` directory do not attempt to thoroughly test
the Mustache implementation underlying a Groom implementation.  For that
we recommend the [Mustache spec](https://github.com/mustache/spec).


Author
------

Groom is authored by [Chris Jerdonek](https://github.com/cjerdonek), the
current [Pystache](https://github.com/defunkt/pystache) maintainer.


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
