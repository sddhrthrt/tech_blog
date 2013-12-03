---
layout: post
title: "Option parsing in Python scripts"
date: 2013-07-16 15:12
comments: true
categories: technology howto
tags: 
 - tutorial 
 - python 
 - optparse 
 - argparse
---

Just wrote a command-line python script and want to spread the happiness to
others by passing it on? If your last hurdle to this is adding ability to take
command line options like those (cool) shell scripts, here is your solution.
<!-- more -->
Now while it is possible to parse the arguments by taking the arguments as
string by doing this,:

``` python Arguments using sys.argv 
import sys 
arguments = sys.argv
# <scriptname> hello -o filename 
# will give sys.argv = [scriptname hello -o filename]
```

the approach is tedious and you will end up more time accounting for various
permutations and combinations.

## ./introduce.py `--`module optprase

Python is definitely a nicer world, and there is `optparse` to your rescue.

__NOTE__: [`optparse`](http://docs.python.org/2/library/optparse.html) is only until python 2.7, in python >3.0, there is
[`argparse`](http://docs.python.org/dev/library/argparse.html).

``` python argparse example
import optparse
#argparse in python >3.0, visit http://docs.python.org/dev/library/argparse.html
parser = optparse.OptionParser()
parser.add_option("-f", "--file",               #the long and short option
                    dest="filename",            #name in the options dict
                    help="write report to FILE",#help text for --help
                    metavar="FILE")             #notice the help text.
parser.add_option("-q", "--quiet",              #again, option names
                    dest="verbose",             #name in options dict
                    action="store_false",       #special way to say, store False
                    default="true",             #default value
                    help="dont print the status messages")
(options, args) = parser.parse_args()           #notice, returned list is unpacked
print options, args                             #refer the output in the next section
#
# ./<scriptname> -f filename -q other-arguments
# ./<scriptname> -qf filename other-arguments
# ./<scriptname> -q -ffilename other-arguments
# ./<scriptname> --quiet --file filename other-arguments
# all these will give an output 
# options                                  args
# {'verbose': False, 'filename': 'myfile'} [other-arguments, ]

```

Before I go any further, let me also show the output of this script.

``` bash argparse output
[pharos_user@pdevdv3os1dv ~]$ python optparser.py -qf myfile other-arguments
{'verbose': False, 'filename': 'myfile'} ['other-arguments']
[pharos_user@pdevdv3os1dv ~]$ python optparser.py --help
usage: optparser.py [options]

options:
-h, --help            show this help message and exit
-f FILE, --file=FILE  write report to FILE
-q, --quiet           dont print the status messages
```

It's apparent that:

*  A `--help` or `-h` option is, rather conveniently, automatically added, which
   prints traditionally styled help text.
*  `help` option to `parser.add_option` provides the help text of each option for the automatically
  produced `--help` usage.
*  `dest` is the variable name in which the option data is stored in the
  `options` dictionary.
*  `action` tells that IF the option is given, then store `False` in the variable
  provided by `dest`.
*  `default`, no surprise here, is the default value for the option.
*  Once we get all the options in the dictionary called options, the remaining
arguments are stored in `args`.

## Footnotes

### Return value unpacking in python

Did you notice how we slickly wrote `(options, args) = parser.parse_args()`, and
two values went into `options` and `args` seperately?

Actually, the function is returning two values in a tuple. If we write a
function like this:

```python multiple return values
def foo():
    return 1, 2, 3
a, b, c = foo()
# a=1, b=2, c=3
```

Got that? Now again, what if you don't want to store `a` and `b`, and just want to
take `c`?

``` python ignore some returns
#both these work:
_, _, c = foo() #ignore first two return values
c = foo()[2]
#which means even this works:
b, c = foo()[1:] #awesome!
```

### Single letter and long options in shell

When a command is called, there can be either a single letter option: `ls -a` or
a long form of the option `ls --all`.

*  Shorter options can be combined together:
`ls -lah` also means `ls -l -a -h`.
*  Option with arguments also can be mixed with
options without: `script -qf filename` is same as `script -q -f filename`.
*  Also, notice how single letter option with argument can be written as `script
-ffilename` without a space between argument and option.


