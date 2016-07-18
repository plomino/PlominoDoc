========
Concepts
========

Plomino database
================

A Plomino *application* is supported by one or more Plomino *databases*.

The Plomino database is the Plone object which contains the application
data (i.e. the Documents_), and its structure (i.e. the
Design_).

Design
======

The *design* of a Plomino application consists of the set of Forms_ and
Views_ provided in the Plomino database.

The design defines the structure of the application, and it is created by
the application designer. It differs from the documents, which are the
application data, created by the users.

Forms
=====

A :dfn:`form` allows users to view and/or to edit information.

A form usually contains some fields of various types (text, date, rich
text, checkbox, attached files, etc.).

The application designer designs the layout he needs for the form, and
inserts the fields wherever he wants.

A form can also contain some action buttons to trigger specific processing.

Forms are not always used to create or view documents |---| sometimes they
are used to provide specific features (see `Search forms`_, and 
`Page forms`_).

Search forms
------------

The application designer can create specific forms dedicated to perform
searches. These forms are not used to create documents, but to input the
search criteria.

It allows the designer to provide more specific and more business-oriented
search features than the global Plone search.

Page forms
----------

The application designer can create page forms to build custom navigation 
menus, generate reports, provide portlet content, etc.


Documents
=========

A :dfn:`document` is a set of data. Data can be submitted by a user using a
given :term:`form`.

.. Note:: a document can be created using one form and then viewed or edited
   using a different form. The presentation of the document is determined
   by the form, which renders the data items found on the document. The
   fields on the form need not correspond one to one with the data items
   stored on the document: there may be more fields, or fewer fields, or
   the type of field may be different. Care should be taken to maintain
   consistency: make sure that the form matches the document. 

   This mechanism allows the document rendering and the displayed action
   buttons to change according to different parameters (user access rights,
   current document state, field values, etc.).

Views
=====

A :dfn:`view` defines a collection of documents.

A view has a *selection formula* which filters the documents that the
application designer wants to be displayed in the view.

A view contains *columns*. :term:`Column` contents is computed from data
stored in the documents.

.. _column:

A Plomino *view* is like a canned search. Views are built up incrementally, 
and not assembled dynamically as would be the case for a catalog query. 
Every time a document is saved, view formulas are evaluated, and if any of
them return ``True``, the document is included in the view.

This has two implications:

- While it is cheap to keep the view up to date incrementally, it can be
  expensive to rebuild views from scratch, as this involves evaluating view
  formulas over all documents in the database.

- A document needs to be saved in order for its state to be reflected in views.
  I.e. a simple ``setItem`` is not enough. 

Views store columns as catalog metadata. This effectively doubles the storage
required for any field when it is added as a view column. It trades space for
speed: the last-saved value of an expensive computed field can be obtained from
the view column without having to execute the field formula again (but watch
out for stale values, if the state or context of the document has changed since
the last save).


Columns
-------

Views can contain columns. The column values are stored and displayed (unless
hidden) for every record that forms part of the view. A :dfn:`column` may refer
to a form field, in which case that field will be used to render the record
value, or it may specify a **column formula**, which need not correspond to one
or any field. 

Column totals
`````````````

Numerical columns can be added up to display column totals (the total for all
the records in the view). If the column refers to a field, that field will 
also be used to render the total.

If desired, column totals can be dynamically computed in the browser per 
view batch. In order to enable this, include the following snippet in the
View's :guilabel:`Dynamic Table Parameters`::

    'fnFooterCallback': generateTableFooter,


.. |---| unicode:: U+02014 .. em dash
