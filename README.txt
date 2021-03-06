=============
Python-GSSAPI
=============

.. role:: python(code)
   :language: python

.. role:: bash(code)
   :language: bash

.. image:: https://travis-ci.org/pythongssapi/python-gssapi.svg?branch=master
    :target: https://travis-ci.org/pythongssapi/python-gssapi

Python-GSSAPI provides both low-level and high level wrappers around the GSSAPI
C libraries.  While it focuses on the Kerberos mechanism, it should also be
useable with other GSSAPI mechanisms.

Requirements
============

Basic
-----

* A working implementation of GSSAPI (such as from MIT Kerberos)
  which includes header files

* a C compiler (such as GCC)

* either the `enum34` Python package or Python 3.4+

Compiling from Scratch
----------------------

To compile from scratch, you will need Cython >= 0.21.1.

For Running the Tests
---------------------

* the `nose` package (for tests)

* the `shouldbe` package (for tests)

Installation
============

Easy Way
--------

.. code-block:: bash

    $ pip install gssapi

From the Git Repo
-----------------

.. code-block:: bash

    $ git clone https://github.com/DirectXMan12/python-gssapi.git
    $ python setup.py build
    $ python setup.py install

Tests
=====

The tests for for Python-GSSAPI live in `gssapi.tests`.  In order to
run the tests, you must have an MIT Kerberos installation (including
the KDC).  The tests create a self-contained Kerberos setup, so running
the tests will not interfere with any existing Kerberos installations.

Structure
=========

Python-GSSAPI is composed of two parts: a low-level C-style API which
thinly wraps the underlying RFC 2744 methods, and a high-level, Pythonic
API (which is itself a wrapper around the low-level API).  Examples may
be found in the `examples` directory.

Low-Level API
-------------

The low-level API lives in `gssapi.raw`.  The methods contained therein
are designed to match closely with the original GSSAPI C methods.  All
relevant methods and classes may be imported directly from `gssapi.raw`.
Extension methods will only be imported if they are present.  The low-level
API follows the given format:

* Names match the RFC 2744 specification, with the :python:`gssapi_`
  prefix removed

* Parameters which use C int constants as enums have
  :python:`enum.IntEnum` classes defined, and thus may be passed
  either the enum members or integers

* In cases where a specific constant is passed in the C API to represent
  a default value, :python:`None` should be passed instead

* In cases where non-integer constants would be used in the API (i.e.
  OIDs), enum-like objects have been defined containing named references
  to values specified in RFC 2744.

* Major and minor error codes are returned by raising
  :python:`gssapi.raw.GSSError`.  The major error codes have exceptions
  defined in in `gssapi.raw.exceptions` to make it easier to catch specific
  errors or categories of errors.

* All other relevant output values are returned via named tuples.

High-Level API
--------------

The high-level API lives directly under :python:`gssapi`.  The classes
contained in each file are designed to provide a more Pythonic, Object-Oriented
view of GSSAPI.  The exceptions from the low-level API, plus several additional
exceptions, live in `gssapi.exceptions`.  The rest of the classes may be
imported directly from `gssapi`.  Only classes are exported by `gssapi` --
all functions are methods of classes in the high-level API.

Please note that QoP is not supported in the high-level API, since it has been
deprecated.

Extensions
----------

In addition to RFC 2743/2744, Python-GSSAPI also has support for:

* RFC 5588 (GSS-API Extension for Storing Delegated Credentials)

* (Additional) Credential Store Extension

* Services4User

* Credentials import-export

The Team
========

(GitHub usernames in parentheses)

* Solly Ross (@directxman12)
* Robbie Harwood (@frozencemetery)
* Simo Sorce (@simo5)
* Hugh Cole-Baker (@sigmaris)
