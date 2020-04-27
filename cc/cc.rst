Altertech Python Coding Convention v1.0.1 (Internal)
====================================================

.. contents::

Code style
----------

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

CamelCase, without separation: *MyClass*

Max line width
--------------

**80** symbols. Wrapping with a reversed slash is allowed after the condition
words:

.. code-block:: python

    if this and _is and a_very and a_long and condition and then and \
        wrapping and with_a and reversed_slash and is_allowed:
            return True

Tabs vs spaces
--------------

No tabs. No. **NO TABS!!!!!!!!!!1111**

1 tab = 4 spaces. It\'s a law.

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

Module imports - one per line

.. code-block:: python

    import os

Object and function imports - multiple allowed

.. code-block:: python

    from time import sleep, time

Overriding internal methods
---------------------------

Allowed for simple plugins, addons, macros.

.. code-block:: python

    # mysimpleplugin.py
    values = {}
    
    def get():
        return values.get('a')

    # replacing "set" structure with a function
    def set():
        values['a'] = 'b'

It's fine to use set() functions in classes, because *self.set is not set*.

Global variables
----------------

Allowed **only** for the simple core modules and config parsers (**only** in
projects started before Jan 2017).

.. code-block:: python

    # config.py

    timeout = 5
    url = 'http://google.com'
    
    def load():
        globals timeout, url
        timeout = 10
        url = 'http://yahoo.com'

We don't consider globals as total evil. But as they're not in trend, it's
much better to use simple namespaces:

.. code-block:: python

    from types import SimpleNamespace

    config = SimpleNamespace(timeout=5, url='http://google.com')

    _d = SimpleNamespace(loaded=False)

    def load():
        config.timeout = 10
        config.url = 'http://yahoo.com'
        _d.loaded = True

String formatting
-----------------

f-string is the most preferred way. "format" is allowed e.g. for complex
conditions, but avoid named formatting (it's slow).

.. code-block:: python

    a = f'{b} {c} {d}'

    # good but
    z = 'b is {}'.format('zero' if b == 0 else 'non-zero')
    # better
    z = f'b is {"zero" if b == 0 else "non-zero"}'
    # but always keep it readable, unless the speed is really important

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

Simple conditions
-----------------

Inline code is always better.

Bad example:

.. code-block:: python

    if a == 1:
        b = 'a is 1'
    else:
        b = 'a is not 1'
    if b: return 'b is set'
    else: return 'b is not set'

Good example:

.. code-block:: python

    b = 'a is 1' if a == 1 else 'a is not 1'
    return 'b is set' if b else 'b is not set'


Background workers
------------------

Avoid starting threads directly, simple wrapper is always better:

.. code-block:: python

    # common wrapper

    class BackgroundWorker:

        def __init__(self, name=None):
            self.__thread = None
            self.__active = False
            self.name = name

        def start(self, *args, **kwargs):
            if not (self.__active and self.__thread and \
                    self.__thread.isAlive()):
                self.__thread = threading.Thread(
                    target=self.run, name=self.name, args=args, kwargs=kwargs)
                self.__active = True
                self.__thread.start()

        def stop(self, wait=True):
            if self.__active and self.__thread and self.__thread.isAlive():
                self.__active = False
                if wait:
                    self.__thread.join()

        def is_active(self):
            return self.__active

    # my worker

    class MyWorker(BackgroundWorker):

        def run():
            while self.is_active():
                # do a job


    worker = MyWorker()
    worker.start()

Development of background workers is preffered with
https://github.com/alttch/neotasker/ library. Example:

.. code-block:: python

    from neotasker import background_worker

    @background_worker
    def myworker(**kwargs):
        print('I\'m a worker ' + kwargs.get('worker_name'))

    myworker.start()

Working with function collections
---------------------------------

https://github.com/alttch/neotasker/ library example, function collection to
shut down the project:

.. code-block:: python

    from neotasker import FunctionCollecton
    
    shutdown = FunctionCollecton()
    
    @shutdown
    def f1():
        print('Stopping stuff #1')
    
    @funcs
    def f2():
        print('Stopping stuff #2')
    
    shutdown.run()

Initializing dictionary values
------------------------------

Always use *setdefault*.

Bad example:

.. code-block:: python

    config = {}
    if 'structure' not in config:
        config['structure'] = {}
    if 'items' not in config:
        config['items'] = []
    config['structure']['a'] = 2
    config['items'].append('item1')


Good example:

.. code-block:: python

    config = {}
    config.setdefault('structure', {})['a'] = 1
    config.setdefault('items', []).append('item1')

CLI tables
----------

https://github.com/alttch/rapidtables

Configs
-------

YAML is preferred. Don't parse YAML directly, use
https://github.com/alttch/pyaltt2

Documentation
-------------

Simple docs: Markdown is preferred

Complex docs: Sphinx/RST is preferred

Web applications
----------------

Engine
~~~~~~

Flask is preferred. REST wrapped for Swagger auto-docs are fine (e.g.
https://github.com/python-restx/flask-restx)

WSGI
~~~~

gunicorn is preferred. Don't use it directly, use
https://github.com/alttch/pyaltt2 app module.

Databases
---------

SQL server
~~~~~~~~~~

PosgreSQL is preferred. Additional support of MySQL and SQLite is highly
welcome.

API
~~~

Don't use SQLAlchemy directly, use https://github.com/alttch/pyaltt2 db module.

SQL queries
~~~~~~~~~~~

Don't keep SQL queries in Python code. Put them to "resources" dir, use
https://github.com/alttch/pyaltt2/tree/master/pyaltt2 res module.

JSON
----

Engine
~~~~~~

https://pypi.org/project/python-rapidjson/ is preferred. Use
https://github.com/alttch/pyaltt2 JSON auto-wrapper.

Validation
~~~~~~~~~~

Always use https://pypi.org/project/jsonschema/ wherever it's possible.

Sending mail
------------

https://github.com/alttch/pyaltt2/tree/master/pyaltt2 mail module is preferred.

Cryptography
------------

https://github.com/alttch/pyaltt2/tree/master/pyaltt2 crypto (Rioja) is
preferred for AES.

CLI color highlighting
----------------------

Usage
~~~~~

Avoid using color functions directly, use wrappers instead. Recommended to
use: https://github.com/alttch/neotermcolor

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
    <div>
        <span style="color: #99CCFF; font-weight: bold">this is blue and bold
        </span>
        <span style="color: #AAAAAA"> = </span>
        <span style="color: yellow">this is yellow and regular</span>
    </div>
    <div>
        <span style="color: #99CCFF; font-weight: bold">this is blue and bold
        </span>
        <span style="color: #AAAAAA"> = </span>
        <span style="color: yellow">but the numbers can be blue and regular
        </span>
    </div>
    </div>

Versions
--------

**major.minor.subversion [alpha|beta]** (*1.0.0 beta*)

Each project should have version at least in the primary file:

.. code:: python

    __version__ = '1.2.3'

Documentation
-------------

Markup
~~~~~~

* **rst (sphinx)** primary
* **md** for the simple texts, but keep it rst-compatible

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

**master** current working branch - unstable code, but at least possible to be
executed

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

Short comments in the stable branches are forbidden.
