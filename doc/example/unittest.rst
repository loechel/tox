unittest2, discover and tox
===============================

Running unittests with 'discover'
------------------------------------------

.. _Pygments: https://pypi.python.org/pypi/Pygments

The discover_ project allows to discover and run unittests
and we can easily integrate it in a ``tox`` run.  As an example,
perform a checkout of Pygments_:

.. code-block:: shell

    hg clone https://bitbucket.org/birkenfeld/pygments-main

and add the following ``tox.ini`` to it:

.. code-block:: ini

    [tox]
    envlist = py25,py26,py27

    [testenv]
    changedir = tests
    commands = discover
    deps = discover

If you now invoke ``tox`` you will see the creation of
three virtual environments and a unittest-run performed
in each of them.

Running unittest2 and sphinx tests in one go
-----------------------------------------------------

.. _`Michael Foord`: http://www.voidspace.org.uk/
.. _tox.ini: https://github.com/testing-cabal/mock/blob/master/tox.ini

`Michael Foord`_ has contributed a ``tox.ini`` file that
allows you to run all tests for his mock_ project,
including some sphinx-based doctests.  If you checkout
its repository with:

.. code-block:: shell

    git clone https://github.com/testing-cabal/mock.git

the checkout has a tox.ini_ that looks like this:

.. code-block:: ini

    [tox]
    envlist = py24,py25,py26,py27

    [testenv]
    deps = unittest2
    commands = unit2 discover []

    [testenv:py26]
    commands =
        unit2 discover []
        sphinx-build -b doctest docs html
        sphinx-build docs html
    deps =
        unittest2
        sphinx

    [testenv:py27]
    commands =
        unit2 discover []
        sphinx-build -b doctest docs html
        sphinx-build docs html
    deps =
        unittest2
        sphinx

mock uses unittest2_ to run the tests. Invoking ``tox`` starts test
discovery by executing the ``unit2 discover``
commands on Python 2.4, 2.5, 2.6 and 2.7 respectively.  Against
Python2.6 and Python2.7 it will additionally run sphinx-mediated
doctests. If building the docs fails, due to a reST error, or
any of the doctests fails, it will be reported by the tox run.

The ``[]`` parentheses in the commands provide :ref:`positional substitution` which means
you can e.g. type:

.. code-block:: shell

    tox -- -f -s SOMEPATH

which will ultimately invoke:

.. code-block:: shell

    unit2 discover -f -s SOMEPATH

in each of the environments. This allows you to customize test discovery
in your ``tox`` runs.

.. include:: ../links.rst
