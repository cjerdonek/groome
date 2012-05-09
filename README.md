Groom
=====

[Groom](http://cjerdonek.github.com/groom/) is a dead-simple,
[Mustache](http://mustache.github.com/)-based standard for project templates.

A groom template is just a directory of files (and subdirectories) containing
a project structure, and an optional directory of Mustache partials.

A groom implementation creates a new project by treating both the name and
contents of each file in the template directory as a Mustache templates.
A [yaml](http://yaml.org/) or [json](http://www.json.org/) configuration
file provides the context to render each Mustache template.


Example
-------

For example, the following groom template--

	partials/
	    copyright.mustache
			# Copyright (C) {{year}} {{author}}.
	template/
		{{project}}.py.mustache
		    {{>copyright}}
			print "Running {{project}}..."

with the following context--

	{
        "project": "awesomeness",
        "author": "Mustachioed Maven",
        "year": 2012
	}

would yield--

	output/
		awesomeness.py
			# Copyright (C) 2012 Mustachioed Maven.
			print "Running awesomeness..."

Rules
-----

Specifically, a groom template directory is rendered as follows:

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

We suggest that groom templates in GitHub be stored in repositories
with names prefixed by `groom-` (for example `groom-python27-script`).
This will make template discovery easier.

In addition, Groom templates can be listed on the project
[wiki](https://github.com/cjerdonek/groom/wiki).

Each groom template should supply a sample configuration file that users
can modify as a starting point.


Implementations
---------------

Groom implementations can also be listed on the wiki.

[Molt](https://github.com/cjerdonek/molt), a Python implementation, is under
construction and should be ready shortly.


Author
------

Groom is authored by [Chris Jerdonek](https://github.com/cjerdonek), the
current [Pystache](https://github.com/defunkt/pystache) maintainer.
