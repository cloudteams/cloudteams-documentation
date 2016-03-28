Frequently Asked Questions
==========================

Do I need an account to view projects?
--------------------------------------

No, viewing the available projects doesn't require an active acccount.

However a CloudTeams customer account is needed in order to follow and actively participate to the projects.

What is the difference between customer and developer account? What account should I create?`
----------------------------------------------------------

We don't support allowing folks to change the slug for their project.
You can update the name which is shown on the site,
but not the actual URL that documentation is served.

The main reason for this is that all existing URLs to the content will break.
You can delete and re-create the project with the proper name to get a new slug,
but you really shouldn't do this if you have existing inbound links,
as it `breaks the internet <http://www.w3.org/Provider/Style/URI.html>`_.

What a Customer account can do?
----------------------------------------------------------

.. note::
    Customer account is needed for following and participating to new projects.

A customer can follow projects and participate to project in an anonymous way.

What a Developer account can do?
----------------------------------------------------------

.. note::
    Developer account is needed for the creation of new projects.


How anonymisation works on CloudTeams? Are my personal data shared to software teams?
----------------------------------------------------------

This happens because our build system doesn't have the dependencies for building your project. This happens with things like libevent and mysql, and other python things that depend on C libraries. We can't support installing random C binaries on our system, so there is another way to fix these imports.

You can mock out the imports for these modules in your ``conf.py`` with the following snippet::

    import sys
    from unittest.mock import MagicMock

    class Mock(MagicMock):
        @classmethod
        def __getattr__(cls, name):
                return Mock()

    MOCK_MODULES = ['pygtk', 'gtk', 'gobject', 'argparse', 'numpy', 'pandas']
    sys.modules.update((mod_name, Mock()) for mod_name in MOCK_MODULES)

Of course, replacing `MOCK_MODULES` with the modules that you want to mock out.

.. Tip:: The library ``unittest.mock`` was introduced on python 3.3. On earlier versions install the ``mock`` library
    from PyPI with (ie ``pip install mock``) and replace the above import::

        from mock import Mock as MagicMock

If such libraries are installed via ``setup.py``, you also will need to remove all the C-dependent libraries from your ``install_requires`` in the RTD environment.

Demo Login (deprecated)
----------------------------------------------

If you'd prefer not to install the use a test user, you can provide these credentials by editing to the login page::

    username = 'test'
    passwrod = 'test'



Where do I need to put my docs for RTD to find it?
--------------------------------------------------

Read the Docs will crawl your project looking for a ``conf.py``. Where it finds the ``conf.py``, it will run ``sphinx-build`` in that directory. So as long as you only have one set of sphinx documentation in your project, it should Just Work.


Image scaling doesn't work in my documentation
-----------------------------------------------

Image scaling in docutils depends on PIL. PIL is installed in the system that RTD runs on. However, if you are using the virtualenv building option, you will likely need to include PIL in your requirements for your project.


How do I support multiple languages of documentation?
-----------------------------------------------------

See the section on :ref:`Localization of Documentation`.

Do I need to be whitelisted?
----------------------------

No. Whitelisting has been removed as a concept in Read the Docs. You should have access to all of the features already.


Can I document a python package that is not at the root of my repository?
-------------------------------------------------------------------------

Yes. The most convenient way to access a python package for example via
`Sphinx's autoapi`_ in your documentation is to use the *Install your project
inside a virtualenv using ``setup.py install``* option in the admin panel of
your project. However this assumes that your ``setup.py`` is in the root of
your repository.

If you want to place your package in a different directory or have multiple
python packages in the same project, then create a pip requirements file. You
can specify the relative path to your package inside the file.
For example you want to keep your python package in the ``src/python``
directory, then create a ``requirements.readthedocs.txt`` file with the
following contents::

    src/python/

Please note that the path must be relative to the file. So the example path
above would work if the file is in the root of your repository. If you want to
put the requirements in a file called ``requirements/readthedocs.txt``, the
contents would look like::

    ../python/

After adding the file to your repository, go to the *Advanced Settings* in
your project's admin panel and add the name of the file to the *Requirements
file* field.

.. _Sphinx's autoapi: http://sphinx-doc.org/ext/autodoc.html
.. _pip requirements file: https://pip.pypa.io/en/stable/user_guide.html#requirements-files

