# lua-pry

`lua-pry` provides an extended interactive prompt that offers more features than the standard Lua interactive mode. It is implemented in pure Lua and depends of [lua-linenoise](https://github.com/nuxlli/lua-linenoise).

This project can be considered a fork of the [ilua](https://github.com/ilua/ilua), with the differentiates readline is replaced by linenoise.

# Instalation

You must have luadist installed.

```bash
$ git clone https://github.com/nuxlli/lua-linenoise
$ luadist make lua-linenoise
```

# Usage

Usage: lua pry.lua [options...]

Features:
- Multi-line input
- Pretty printing of tables and other values.
- Displays numeric index tables, key/value tables, and mixtures of both. 
- Tables are printed sorted by key. The integer index part of the table
  is treated as an array, and printed without keys.
- Session logging (-t and -T options)
- Helper variables/functions (-H option)
- Runs in a non global environment (however required and loaded files will
  currently run in global environment)

QUICK START
-----------

-------------------------------------------------------------------------------
$ lua pry.lua 
PRY: Lua 5.1

>> {1,2,3,4,5,6,7,8}
{ 1, 2, 3, 4, 5, 6, 7, 8 }

>> {1,2,3,4,5,6,z="foo",h="bar",b="baz",7,8,9,[-10]=-10,[-9]=-9,[15]=15,[16]=16}
{
    [-10] = -10, [-9] = -9, 1, 2, 3, 4, 5, 6, 7, 8, 9, [15] = 15, [16] = 16, 
    b = "baz", h = "bar", z = "foo"
}

>> pry
{
    chunkname = "stdin", 
    collisions = { }, 
    declared = {
        Pry = true, Pretty = true, _ = true, dir = true, pry = true, 
        ls = true, p = true, slice = true
    }, 
    dir = {
        bl = "{ ", bl_m = "{\n", br = " }", br_m = "\n%s{i}}", compact = false, 
        delim1 = ", ", delim2 = ", ", depth = 1, empty = "{ }", eol = "\n", 
        eq = " = ", field = "%s", function_info = true, indent1 = "    ", 

.... cut due to long length of output ....

>> 5 * 6
30

>> _
30

>> dir(pry.env)
table: 0x10010e970 {
    Pry                 = >>>, 
    Pretty               = >>>, 
    _                    = 30, 
    dir                  = >>>, 
    pry                 = >>>, 
    ls                   = >>>, 
    p                    = >>>, 
    slice                = function: 0x100113180
}

>> ls(_G)
{
    _G = <<  >>, _VERSION = "Lua 5.1", arg = >>>, assert = function, 
    collectgarbage = function, coroutine = >>>, debug = >>>, dofile = function, 
    error = function, gcinfo = function, getfenv = function, 
    getmetatable = function, io = >>>, ipairs = function, load = function, 
    loadfile = function, loadstring = function, math = >>>, module = function, 
    newproxy = function, next = function, os = >>>, package = >>>, 
    pairs = function, pcall = function, print = function, rawequal = function, 
    rawget = function, rawset = function, readline = >>>, require = function, 
    select = function, setfenv = function, setmetatable = function, 
    string = >>>, table = >>>, tonumber = function, tostring = function, 
    type = function, unpack = function, xpcall = function
}
-------------------------------------------------------------------------------

"dir(pry.env)" shows the objects available in the ilua environment. "dir"
is a pretty print object, an instance of the Pretty class. There are two
other pretty print objects: "ls" and "p". "ls" is similar to "dir" with a
depth limit of 1, but with compact formatting. "p" is the standard pretty
printer used for printing interactive results. Pretty objects can be used
as functions, in which case they print their arguments.

Note: the ">>>" in the table display means there is more depth to the table
than shown, or more specifically, that the pretty printing recursion depth
limit has been exceeded. The dir object has a depth limit of one (this is
intentional)

Within the pry environment, you can access the ilua object itself via
"pry". The last result is available in the special variable "_". The
classes Pry and Pretty are also available, as well as an array slice
function you may find handy to display parts of long arrays.

Here is a breakdown of the default objects in the pry environment: 
    Pry                  = >>>,                     Pry class
    Pretty               = >>>,                     Pretty class (pretty printing)
    _                    = 30,                      variable containing last interactive result
    dir                  = >>>,                     Pretty instance
    pry                  = >>>,                     Pry instance
    ls                   = >>>,                     Pretty instance
    p                    = >>>,                     Pretty instance
    slice                = function: 0x100113180    array slice function


MORE INFORMATION
----------------

You can learn a lot about pry by printing and experimenting with the ilua
objects. Pretty printing can be configured by setting parameters within the
pretty printing objects (see source for descriptions of the parameters). To
change the default printing, set parameters of the "p" object. The "pry"
object also has some settable parameters. See the source code for more
information.

TODO: more documentation

Hint: try running pry with the -H option to inject a few shortcut/helpers
into the enviroment. You can also call "pry:helpers()" to do the same thing.

COPYRIGHT / LICENSE
-------------------

[ilua](https://github.com/ilua/ilua) is based on work by Steve Donovan. The original ILUA documentation is at: http://lua-users.org/wiki/InteractiveLua 

See LICENSE for copyright and license information.

