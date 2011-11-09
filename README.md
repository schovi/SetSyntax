SetSyntax
=========

SetSyntax allows you to map filenames that do not have an extension with a syntax. For example, Cappuccino uses files called "Jakefile" that should be associated with the JavaScript syntax, but because there is no filename extension, the syntax is not automatically set.

### Configuration

To configure SetSyntax, add a map called "set\_syntax\_map" to your user File Settings, like this:

    "set_syntax_map":
    {
        "Jakefile": "JavaScript"
    }

Each item in the map consists of a name and a syntax mapping. If the name does not begin with "#!", it signifies an exact filename (case-sensitive) that you want to match. In the example above, the exact filename "Jakefile" will map to the JavaScript syntax.

If the name begins with "#!", it signifies the *target executable* of a shebang in an executable shell script. The *target executable* is the last name component in the shebang. For example, all of the following shebangs target the executable "python":

    #!/usr/bin/python
    #!/usr/local/bin/python
    #!python
    #/usr/bin/env python
    #/usr/env python

ST2 is pretty smart about figuring out the syntax of an executable shell script from the shebang. But in those cases where it cannot determine the syntax, SetSyntax gets a chance. For example, suppose you open an executable shell script called "apply_fix", which uses the Objective-J interpreter `objj`. The script looks like this:

    #!/usr/bin/env objj

    @import <Foundation/Foundation.j>
    @import <AppKit/AppKit.j>

    var FILE = require("file"),
        OS = require("os");

    function main(args)
    {
        // do something here
    }

We want any shell scripts that target `objj` to use the "Objective-J" syntax, so we add an item to our syntax map:

    "set_syntax_map":
    {
        "Jakefile": "JavaScript",
        "#!objj": "Cappuccino/Objective-J"
    }

There are two things of note in the second item:

* The target executable name should immediately follow the "#!" with no space.
* The syntax name consists of two parts separated by "/". If the syntax name contains "/", the name before "/" is a package name and the name after "/" is the syntax name (without the ".tmLanguage" extension) within that package to use. In the case of the "Cappuccino" package, there is no "Cappuccino" syntax, so we must specify the name of the syntax.
