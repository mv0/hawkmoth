.. _extension:

C Autodoc Extension
===================

Hawkmoth provides a Sphinx extension that adds a new directive to the Sphinx
:any:`C domain <sphinx:c-domain>` to incorporate formatted C source code
comments into a document. Hawkmoth is Sphinx :any:`sphinx:sphinx.ext.autodoc`
for C.

For this to work, the documentation comments must of course be written in
correct reStructuredText. See :ref:`documentation comment syntax <syntax>` for
details.

Installation
------------

Add ``hawkmoth`` to :data:`sphinx:extensions` in ``conf.py``. Note that
depending on the packaging and installation directory, this may require
adjusting the :envvar:`python:PYTHONPATH`.

Configuration
-------------

The extension has a few configuration options that can be set in ``conf.py``:

.. py:data:: cautodoc_root

   Path to the root of the source files. Defaults to the
   :term:`sphinx:configuration directory`, i.e. the directory containing
   ``conf.py``.

   To use paths relative to the configuration directory, use
   :func:`python:os.path.abspath`, for example:

   .. code-block:: python

      import os
      cautodoc_root = os.path.abspath('my/sources/dir')

.. py:data:: cautodoc_compat

   Compatibility option. One of ``none`` (default), ``javadoc-basic``,
   ``javadoc-liberal``, and ``kernel-doc``. This can be used to perform a
   limited conversion of Javadoc-style tags to reStructuredText.

   .. warning::

      The compatibility options and the subset of supported syntax elements
      are likely to change.

.. py:data:: cautodoc_clang

   A comma separated list of arguments to pass to ``clang`` while parsing the
   source, typically to define macros for conditional compilation, for example
   ``-DHAWKMOTH``. No arguments are passed by default.

Directive
---------

This module provides the following new directive:

.. rst:directive:: .. c:autodoc:: filename-pattern [...]

   Incorporate documentation comments from the files specified by the space
   separated list of filename patterns given as arguments. The patterns are
   interpreted relative to the :data:`cautodoc_root` configuration option.

   The ``compat`` option overrides the :data:`cautodoc_compat` configuration
   option.

   The ``clang`` option overrides the :data:`cautodoc_clang` configuration
   option.

Examples
--------

The basic usage is:

.. code-block:: rst

   .. c:autodoc:: interface.h

Several files with compatibility and compiler options:

.. code-block:: rst

   .. c:autodoc:: api/*.[ch] interface.h
      :compat: javadoc-basic
      :clang: -DHAWKMOTH
