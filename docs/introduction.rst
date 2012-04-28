------------
Introduction
------------

The main objective of Plomino is to provide the ability to build
business-specific applications in Plone without requiring development of
Plone extension products.

Plomino allows you to create forms, to use those forms to view or edit
structured contents, to filter and list those contents, to perform
search requests, to add business-specific features and to automate
complex processing -- all entirely through the Plone web interface.

.. Note:: 
    Plomino is widely inspired by the IBM Lotus Domino (tm) commercial
    product, it reproduces its main concepts and features, and it uses
    its terminology (which sometimes overlaps with the Plone
    terminology).

Plomino is used in deployments with over 50 000 documents. Users include 
the UN, European banks and local government organizations.

Most Plomino users are Plone users who are not Plone developers (or just
beginners), who, once they have built a nice website with Plone, find that
they are not able to develop specific features using the standard Plone
frameworks.

And a smaller part are actual Plone developers who appreciate Plomino
because it is extremely flexible or because they want their customers to
be more autonomous once they have delivered their work.

Plomino derives various benefits from existing as a Plone add-on.

In the first place, Plone provides a wonderful framework, including key
components for Plomino (ZCatalog, zope security, PythonScripts, ...), and
Plone also provides very useful features (CMS features, user management,
skinning, etc.).  

But the main advantage is the pluggability of Plone. Plone is the only major
framework which provides real pluggability.  This is a wonderful advantage
because it allows Plomino to benefit from excellent Plone products very
easily. 

.. sidebar:: Pluggability vs. extensibility 
   For more on this topic, see
   this excellent post by 
   `Chris McDonough on Pyramid's extensibility
   <http://groups.google.com/group/pylons-discuss/msg/b19df600ddb8be3f>`_,
   along with `Paul Everitt's commentary
   <http://pauleveritt.wordpress.com/2011/01/14/chris-mcdonough-on-pluggable-apps/>`_.
   `Eric Bréhault
   <http://www.makina-corpus.org/blog/quel-prix-devient-vraiment-pluggable>`_
   provides a wider perspective.
