Reverse engineering framework in python


## What is Miasm? ##
Miasm is a a free and open source (GPLv2) reverse engineering framework.
Miasm aims at analyzing/modifying/generating binary programs. Here is
a non exhausting list of features:
  * opening/modifying/generating PE/ELF 32/64 le/be using Elfesteem
  * Assembling/Disassembling ia32/ppc/arm
  * Representing assembly semantic using intermediate language
  * Emulating using jit (dynamic code analysis, unpacking, ...)
  * Expression simplification for automatic de-obfuscation
  * ...

## How does it work? ##
Miasm embed its own disassembler, intermediate language and
instruction semantic. It is written in Python.

To emulate code, it uses libtcc or llvm to jit C code generate from intermediate
representation. It can emulate shellcodes, parts of binaries. Python callback
can be executed to emulate library functions.

## Documentation ##
TODO

## Obtain Miasm ##
clone repo: http://code.google.com/p/miasm/
```
hg clone https://code.google.com/p/miasm/
```
## Software requirements ##
Miasm uses:

  * libtcc http://repo.or.cz/w/tinycc.git to Jit code for emulation mode. see below

  * or llvm v3.2 with python-llvm, see below

  * python-pyparsing

  * python-dev

  * elfesteem from http://code.google.com/p/elfesteem/

## Configuration ##
  * Install elfesteem
```
$ hg clone https://code.google.com/p/elfesteem/
$ cd elfesteem_directory
$ python setup.py build
$ sudo python setup.py install
```
  * To use the jitter, tcc or llvm is mandatory
  * The libtcc needs a little fix in the makefile:
    * remove libtcc-dev from the system to avoid conflicts
    * clone tinycc release\_0\_9\_26
http://repo.or.cz/w/tinycc.git/snapshot/d5e22108a0dc48899e44a158f91d5b3215eb7fe6.tar.gz
    * edit makefile
    * add option -fPIC to the CFLAGS definition: CFLAGS+= -fPIC
```
#
# Tiny C Compiler Makefile
#

TOP ?= .
include $(TOP)/config.mak
VPATH = $(top_srcdir)

CPPFLAGS = -I$(TOP) # for config.h

# ADD NEXT LINE:
CFLAGS+= -fPIC
...
```
    * ./configure && make && make install
  * llvm
    * Debian (testing/unstable): install python-llvm
    * Debian stable/Ubuntu/Kali/whatever: install from http://www.llvmpy.org/
    * Windows: python-llvm is not supported :/
  * Build and install Miasm:
```
$ cd miasm_directory
$ python setup.py build
$ sudo python setup.py install
```

If something goes wrong in the jitter compilation, miasm will skip the error and disable the feature.

## Testing fresh install ##
```
cd miasm_directory/test
python test_all.py
```

Multi cpu is not uspported:
```
cd miasm_directory/test
python test_all.py -m
```

## Misc ##
  * Man, does miasm has a link with rr0d?
  * Yes! crappy code and uggly documentation.