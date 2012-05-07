Groom
=====

[Groom](https://github.com/cjerdonek/groom) is a dead-simple,
[Mustache](http://mustache.github.com/)-based standard for project templates.

A groom template is just a directory of files.  Given a groom template and
a [yaml](http://yaml.org/) or [json](http://www.json.org/) configuration file,
a groom implementation creates a project directory from that directory by
treating the files and file names as Mustache templates and using the
configuration file as the initial context.


Rules
-----

Specifically,

1.  All file names and directory names are treated as Mustache templates.
    For example, `{{project}}.py` might become `molt.py`.
2.  If a file name ends in `.mustache`, the contents of that file are treated
    as a Mustache template and the trailing `.mustache` stripped from the
    file name.
3.  The previous rule has an exception to leave the contents of a `.mustache`
    file alone.  If a file name ends in `.skip.mustache`, the file contents
    remain unchanged, and the trailing `.skip.mustache` is changed to
    `.mustache`.
4.  If a file name does not end in `.mustache`, its contents remain the same.


Templates
---------

We suggest that groom templates in GitHub be stored in repositories
with names prefixed with `groom-` (for example `groom-python27-script`).
This will make template discovery easier.

In addition, Groom templates can be listed on the project [wiki](https://github.com/cjerdonek/groom/wiki.)


Implementations
---------------

Groom implementations can also be listed on the wiki.

A Python implementation is under construction.


Author
------

Groom is authored by [Chris Jerdonek](https://github.com/cjerdonek), the
current [Pystache](https://github.com/defunkt/pystache) maintainer.
