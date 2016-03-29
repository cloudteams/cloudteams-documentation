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
.. Tip:: If you'd prefer not to install the use a test user, you can provide these credentials by editing to the login page::

    username = 'test'
    passwrod = 'test'

After adding the file to your repository, go to the *Advanced Settings* in
your project's admin panel and add the name of the file to the *Requirements
file* field.

.. _Sphinx's autoapi: http://sphinx-doc.org/ext/autodoc.html
.. _pip requirements file: https://pip.pypa.io/en/stable/user_guide.html#requirements-files

