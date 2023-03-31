Install Sphinx
=====================================

Requirement
-------------
    git make python3 python3-pip

Install
-------------
    ``pip3 install sphinx sphinx-autobuild sphinx_rtd_theme sphinxcontrib-plantuml``

Directory Structure
---------------------
    * ``Makefile``: Building documents using ``make html``.
    * ``build``: the output directory of the generated files.
    * ``make.bat``: Only windows uses the command line.
    * ``source/_static``: Static file directory, such as pictures.
    * ``source/_templates``: Template directory.
    * ``conf.py``: Stores Sphinx configurations, including those selected during sphinx quickstart. You can define other values yourself.
    * ``index.rst``: The document project start file.

reStructuredText
------------------
    `reStructuredText Introduction <http://doc.yonyoucloud.com/doc/zh-sphinx-doc/rest.html>`_
