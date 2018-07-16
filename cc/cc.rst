Altertech Python Coding Convention v1.0.0
=========================================

.. contents::

Code style:
-----------

Google. https://github.com/google/styleguide/blob/gh-pages/pyguide.md

(set your autoformatter to code style: **google**).

Functions, variables
--------------------

always lowercase, words separated with **_**: *my_variable*, *my_function*.

Function params
---------------

* **internal functions** as detailed as possible (*format_language="python"*)

* **API functions** as short as possible (*f="python"*).

Classes
-------

First letters of the words - uppercase, without separation: *MyClass*

Max line width
--------------

**80** symbols. Wrapping with a reversed slash is allowed after the condition
words:

.. code-block:: python

    if this and _is and a_very and a_long and condition and then and \
        wrapping and with_a and reversed_slash and is_allowed:
            return True

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

Overriding internal methods
---------------------------

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

Both old style ((*'s: %s' % s*) and new style (*'s: {}'.format(s)*) are allowed,
new style is preferred.

File names
----------

All lowercase, words separated with **-** for executable (*my-tool*), with **_**
for modules (*my_module.py*)

String packing
--------------

* **Dict fields** separated with **,** (*"var1=1,var2=2"*)
* **Lists** separated with **|** (*"1|2|3"* = *[1,2,3]*)
* **Complex arrays** separated with **||** (*"1|2||3|4"* = *[ [1,2], [3,4] ]*)

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
* Double quotes for the multi line strings

.. code-block:: python

    a = """
    this is a very long string
    and we use double quotes
    """

CLI color highlighting
----------------------

Usage
~~~~~

Avoid using color functions directly, use wrappers instead:

.. code-block:: python

    # this is a bad example
    def func_bad(self):
        print(termcolor.colored('my text', color='green'))

    # this one is good
    def func_good(self):
        print(self.colored('my text', color='green'))

    def colored(self, text, color=None, on_color=None, attrs=None):
        return text if self.suppress_colors else \
            termcolor.colored(text, color=color, on_color=on_color, attrs=attrs)


Messages
~~~~~~~~

* **DEBUG** grey and bold
* **INFO** regular
* **WARNING** yellow
* **ERROR** red
* **CRITICAL** red and bold

.. raw:: html

    <div style="padding: 15px; background-color: black">
        <div style="color: #777777; font-weight: bold">DEBUG MESSAGE</div>
        <div style="color: #AAAAAA">INFO MESSAGE</div>
        <div style="color: yellow">WARNING MESSAGE</div>
        <div style="color: red">ERROR MESSAGE</div>
        <div style="color: red; font-weight: bold;">CRITICAL MESSAGE</div>
    </div>

Tables
~~~~~~

.. raw:: html

    <div style="padding: 15px; background-color: black">
        <div style="color: #99CCFF">this is a header, blue and regular</div>
        <div style="color: #777777">---- this is separator, it's grey ----</div>
        <div style="color: #AAAAAA">TABLE CONTENT</div>
    </div>


Structures
~~~~~~~~~~

Both JSON and regular output:

.. raw:: html

    <div style="padding: 15px; background-color: black">
        <span style="color: #99CCFF; font-weight: bold">this is blue and bold
        </span>
        <span style="color: #AAAAAA"> = </span>
        <span style="color: yellow">this is yellow and regular</span>
    </div>

Versions
--------

**major.minor.subversion [alpha|beta]** (*1.0.0 beta*)

Documentation
-------------

Markup
~~~~~~

* **rst (sphinx)** primary
* **md** for the simple texts

Formatting
~~~~~~~~~~

For the lists of functions, commands, variables etc:

* **func1** this is field one
* **func2** this is field two

For the simple lists:

* This is a simple list
* and it\'s field #2

Font styles:

* Function names, file names, variables, single characters: **bold**
* Examples, values: *italic*

Example:

    The variable **var1** contains a values separated with **|** returned by
    function **func1** with **param1** set to *False*, i.e.:
    *func1(param1=False)*

Max line width
~~~~~~~~~~~~~~

**80** symbols, everywhere it is possible.

Version control
---------------

Primary VCS
~~~~~~~~~~~

git

Branches
~~~~~~~~

**master** current working branch - unstable code

**<version>**  i.e. *1.0.0* - stable branch

**all_other_names** upload whatever you wish, separate name words with **__**,
keep it lowercase.

Commits
~~~~~~~

Short comments like *fixes*, *formatting* are allowed, but only for the short
and clear code or documentation changes:

.. code-block:: python

    #commit bf9aafe901e52c5e0834dab45cecf2550b50934e: initial
    a=a-'2'
    #commit ae1aafe901e52c5e0834dab45cecf2550b50934a: fix
    a=a-2
    #commit e1d828306b275471e65940bd063d5d472ceb1cf7: fmt
    a = a - 2
