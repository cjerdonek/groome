Groom
=====

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

For example, the following Groom template--

	partials/
	    copyright.mustache
			# Copyright (C) {{year}} {{author}}.
	template/
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
    are not treated as a Mustache template.


Templates
---------

If you make a Groom template for others to use, you can list it on the Groom
project [wiki](https://github.com/cjerdonek/groom/wiki).

To simplify Groom template distribution, use, and discovery, we suggest that
Groom template projects follow these conventions:

* Store groom templates in repositories with names prefixed by `groom-`
  (for example `groom-python2and3-script`).
* Name the partials directory `partials`.
* Provide an example configuration file named `sample.json` or `sample.yaml`
  that contains a name-value collection.
* The rendering context should be the value of the key `context` in the
  configuration file.  This allows for the inclusion of additional metadata
  in the configuration file, without risk of key collisions with context data.


Implementations
---------------

Groom implementations can also be listed on the wiki.

[Molt](https://github.com/cjerdonek/molt), a Python implementation, is under
construction and should be ready shortly.


Author
------

Groom is authored by [Chris Jerdonek](https://github.com/cjerdonek), the
current [Pystache](https://github.com/defunkt/pystache) maintainer.
