Groom
=====

[Groom](http://cjerdonek.github.com/groom/) is a dead-simple,
[Mustache](http://mustache.github.com/)-based standard for project templates.

A groom template is just a directory of files (and subdirectories).

A groom implementation creates a project directory by treating both the
names of files in the directory and their contents as Mustache templates.
A [yaml](http://yaml.org/) or [json](http://www.json.org/) configuration
file provides the initial context.


Example
-------

TODO: mention partials directory.
TODO: mention context key.
TODO: file determination should be made before rendering file name.


Rules
-----

Specifically,

1.  All file names and directory names are treated as Mustache templates.
    For example, `{{project}}.py` might become `molt.py`.
2.  If a file name ends in `.mustache`, the trailing `.mustache` is stripped
    from the file name and the file contents treated as a Mustache template.
3.  To leave a Mustache file alone, end the file name in `.skip.mustache`.
    For such files, the file contents remain unchanged, and the trailing
    `.skip.mustache` is changed to `.mustache`.  This is the only exception
    to the previous rule.
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

TODO: suggested name for sample configuration file?


Implementations
---------------

Groom implementations can also be listed on the wiki.

[Molt](https://github.com/cjerdonek/molt), a Python implementation, is under
construction and should be ready shortly.


Author
------

Groom is authored by [Chris Jerdonek](https://github.com/cjerdonek), the
current [Pystache](https://github.com/defunkt/pystache) maintainer.
