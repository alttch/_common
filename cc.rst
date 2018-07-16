Altertech Python Coding Convention v.1.0.0
==========================================

Code style:
----------

Google. https://github.com/google/styleguide/blob/gh-pages/pyguide.md

(set your autoformatter to code style: **google**).

Functions, variables
--------------------

always lowercase, words separated with **_**: *my_variable*, *my_function*.

Function params
---------------

* **internal functions** as much as possible details
  (*format_language="python"*)

* **API functions** as short as possible (*f="python"*).

Classes
-------

First letters of the words: uppercase, without separation: *MyClass*

Max line width
--------------

**80** symbols.

Single line code, multi line code
---------------------------------

.. code-block:: python

    # regular code: avoid
    var1 ='a'; print(a); return True

    # structures: possible when short and clear for understanding
    if a: a = 1
    try: a = int(a)
    except: a = None

Imports
-------

One per line

.. code-block:: python

    import os
    from time import sleep

Internal methods override
-------------------------

Allowed for a simple plugins, addons, macros

.. code-block:: python

    # mysimpleplugin.py
    values = {}
    
    def get():
        return values.get('a')

    # replacing "set" structure with a function
    def set():
        values['a'] = 'b'

Global variables
----------------

Allowed **only** for the simple core modules and config parsers

.. code-block:: python

    # config.py

    timeout = 5
    url = "http://google.com"
    
    def load():
      globals timeout, url
      timeout = 10
      url = "http://yahoo.com"

String formatting
-----------------

Both old style ((*'s: %s' % s*) and new style (*'s: {}'.format(s)*) allowed,
new style is preferred.

File names
----------

All lowercase, words separated with **-** for executable (*my-tool*), with **_**
for modules (*my_module.py*)

String packing
--------------

**Dict fields** separated with **,** (*"var1=1,var2=2"*)
**Lists** separated with **|** (*"1|2|3"* = *[1,2,3]*)
**Complex arrays** separated with **||** (*"1|2||3|4"* = *[ [1,2], [3,4] ]*)

Function comments
-----------------

Google-style:

.. code-block:: python

    def function_with_pep484_type_ann(p1: int, p2: str) -> bool:
        """Example function with PEP 484 type annotations.
    
        Args:
            p1: The first parameter.
            p2: The second parameter.
    
        Returns:
            The return value. True for success, False otherwise.
    
        """

Quotes
------

* Single quotes (**'**) everywhere: *myvar = 'my value'*
* Double quotes for the multiline strings

.. code-block:: python

    a = """
    this is a very long string
    and we use double quotes
    """

Versions
--------

**major.minor.subversion [alpha|beta]** (*1.0.0 beta*)

Documentation
------------

Markup
~~~~~~

* **rst (sphinx)** primary
* **md** for the simple texts

Formatting
~~~~~~~~~~

For the lists of functions, fields, variables etc:

* **field1** this is field one
* **field2** this is field two

For the simple lists:

* This is a simple list
* and it\'s field #2

Font styles:

* Function names, file names, variables, single characters: **bold**
* Examples, values: *italic*

Example:

    "The **variable** contains a values separated with **|** returned by
    function **func1** with **param1** set to *False*, i.e.:
    *func1(param1=False)*"

Max line width
~~~~~~~~~~~~~~

**80** symbols, everywhere it is possible.

