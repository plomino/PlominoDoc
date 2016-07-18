-------------------
Installation
-------------------

Prerequisites
-------------

Plomino is built on Plone, so in order to install Plomino, you first need to
install Plone: go to http://plone.org, download Plone and follow the
instructions.

Deploy Plomino in your Plone site
---------------------------------

To deploy the Plomino product, you need to edit your ``buildout.cfg`` file
and add the following in the ``eggs`` section:

.. code-block:: ini

    eggs =
         ...
         Products.CMFPlomino

Then you have to run ``buildout`` to realize your configuration:

.. code-block:: sh

    bin/buildout -N

This will download the latest Plomino version (and its dependencies) from
the http://pypi.python.org/ repository and deploy it in your Zope instance.

Now you can restart your Zope instance and in your Plone site, go to 
:guilabel:`Site setup` / :guilabel:`Add-on products`.

Here you should see ``Plomino`` and ``plomino.tinymce`` in the list of
installable products. Select them and click :guilabel:`Install`.

Once done, Plomino is installed, so when you are in a folder, you can add a 
new Plomino database using the Plone :guilabel:`Add new...` menu.

Deploy Plomino development version
----------------------------------

.. code-block:: sh

    $ virtualenv --no-site-packages --distribute ./venv
    $ cd ./venv
    $ source bin/activate
    $ git clone git://github.com/plomino/Plomino.git
    $ cd ./Plomino
    $ python bootstrap.py
    $ bin/buildout -N