==================================
Build a simple Plomino application
==================================

Create a Plomino database
=========================

To create a Plomino database, select ``Plomino: database`` in the 
:guilabel:`Add item` Plone menu.

.. image:: images/m440f207a.png

Enter a title for the database (for instance *Library*) and save it.

Add a form
==========

Forms can be added using the Plomino Design portlet, which is usually
displayed in the left-hand column, or using the Plone :guilabel:`Add`
dropdown menu.

To add a form, click :guilabel:`Add new... Form` in the portlet, or select
``Plomino: form`` from the :guilabel:`Add item` Plone menu.

.. image:: images/m4539359f.png

On the add form, enter the form id. The form id is initialized with a
generated value (for instance: ``plominoform.2008-01-31.9797530894``). It is
preferable to replace it with a more meaningful id (for instance:
``frmBook``). It is a technical identifier, so use basic characters and
numbers only (blank space and special characters are forbidden).

In the :guilabel:`Title` field, enter the form label, which will be
displayed to the users (for instance: ``Book description``.

.. image:: images/m34f61fc7.png

Save the form to create it (you need to save it before being able to add
fields to your form).

Create the layout and add fields
================================

Click on the :guilabel:`Edit` tab.

Go to the :guilabel:`Form layout` section which contains the TinyMCE editor.
If necessary, expand the editing area by dragging the bottom-right corner,
or clicking on the full-screen icon from the editor toolbar.

Create your form layout using the standard editing tools (styles, tables,
etc.).

.. image:: images/build-1.png

To add a field to the layout, select some word in the layout and click on
the :guilabel:`Add/edit Plomino field` button in the TinyMCE toolbar. 

.. image:: images/build-2.png

The selected text will be used as the field id, and a pop-up window will
allow you to enter the field main parameters:

.. image:: images/build-3.png

For the  ``bookAuthor`` field, keep the default values ('Text' and
'Editable'), click :guilabel:`Insert` and then :guilabel:`Close`.

As you can see, the field is rendered with a blue dashed border in the
layout.

Do the same for the following fields:

- ``bookTitle``, type 'Text', 'Editable' 
- ``publicationYear``, type 'Number', 'Editable' 
- ``summary``, type 'Rich text', 'Editable' 
- ``cover``, type 'File attachment', 'Editable'
- ``bookCategory``, choose type 'Selection list', 'Editable', but after
  clicking :guilabel:`Insert`, click on :guilabel:`Specific settings`.

This opens the field settings page in a new window, where you can enter the
possible values for the Selection list: 

.. image:: images/build-4.png

Click :guilabel:`Apply`, go back to the Form window, and close the field
pop-up.

Now the form is built, and its associated fields have been created.

.. image:: images/build-5.png

Save the form (click the :guilabel:`Save` button at the bottom of the page).

Use the form
============

You can now use this form to create documents.

.. image:: images/build-6.png

Go back to the *Library* database. The database welcome page now contains a
link to add a new document using the ``Book description`` form:

.. image:: images/build-7.png

Click on this link, and you get the form displayed as designed in the
TinyMCE editor, including the fields as they have been defined: 

.. image:: images/build-8.png

You can enter values and save, and a new document will be created: 

.. image:: images/build-9.png 


Explore the database design
===========================

Go to the *Library* database and click the :guilabel:`Design` tab.

This tab displays all the design elements contained in the database: 

.. image:: images/build-10.png 

The pencil icon gives access to the corresponding object in edit mode,
the page icon in read mode, and the folder icon in content mode.

Change the document title
=========================

By default, all the documents created with a form have the same title as the
form. 

In the present case, the title is "Book description", and it will be the
title of all the documents you would create with your form.

To display a more useful title, go to the ``frmBook`` object, edit it, and
enter the following formula in the :guilabel:`Document title formula` field::

    return "Information about %s (%s)" % (
        plominoDocument.getItem('bookTitle'), 
        plominoDocument.getItem('bookAuthor'))

Save the form, go back to the document, make a change and save it. This
will trigger calculation of the title formula. Now you will see the title
has been set as specified in the formula: 

.. image:: images/build-11.png

The document title is computed by a formula. All Plomino formulas are 
restricted Python scripts with certain variables and functions provided.
In this case, the ``plominoDocument`` variable is used, which is the current
document.

All the data items stored on the document by forms, or set using formulas, 
are accessible using the ``getItem`` API: 
(``plominoDocument.getItem(<field name>)``).

For more information about formulas, see Formulas_ below.

.. _Formulas: ./features.html#Formulas

Change the document id
======================

The document id is used in the URL. By default, it is an opaque random
identifier (``4e219e4ffff21b9753c94a0e006e95bf`` in the following)::

    http://localhost:8090/demo/books/plomino_documents/4e219e4ffff21b9753c94a0e006e95bf

If you want to use meaningful ids, you can define a :guilabel:`Document id
formula`.  Go to the ``frmBook`` object, edit it, and enter the following
formula in :guilabel:`Document id formula`::

    plominoDocument.bookTitle +"-"+plominoDocument.bookAuthor

Unlike the title, the id is computed at creation time, and it cannot be
changed later.  So the existing document will not use this formula even if
we re-save it.  But if you create a new document, you will get a id
corresponding to your formula::

    http://localhost:8090/demo/books/plomino_documents/1919-john-dospassos

.. Warning:: If you use this facility, you need to take care that document
   ids are unique, well-formed, and resolve any issues that arise when 
   replicating documents to other Plomino instances. Calculating your 
   own document ids can be a considerable responsibility, depending on the
   requirements of your application.


Add a view
==========

A :term:`view` defines a collection of documents. Some views are used to
present lists of documents to users, and some are used from formulas to
structure the Plomino application. 

A view has a selection formula, which defines which documents form part of
the view, and it usually contains some columns to display information about
the matching documents. These columns may compute derived information from
data items on documents, or even from values looked up from other documents,
Plone objects, or other sources. 

You can generate a view automatically from a form:

- Go to the ``frmBook`` form, and 
- click on :guilabel:`Generate view` in the :guilabel:`Design` portlet on
  the left.

This generates a view which:

- selects all the documents that were created or last edited using the
  ``frmBook`` form,
- creates a :guilabel:`column` for each field on the form (file attachments
  and rich text fields are skipped), and it also 
- inserts an :guilabel:`Add new` action.

.. image:: images/build-12.png

The columns can be re-ordered by drag-and-drop in the :guilabel:`Contents`
tab. The column labels can also be changed.


Add a view manually
===================

Go back to the *Library* database.

Select ``Plomino: view`` from the :guilabel:`Add item` Plone menu. Enter an
identifier (``allBooks``) and a title ('All the books'):

.. image:: images/m57ed2659.png

Enter a selection formula too: this formula must return ``True`` or
``False``. It is evaluated for each document; if the returned value is
``True``, the document is included in the view; if ``False``, it is
rejected.

Enter the following expression and hit :guilabel:`Save`::

    return True

(this expression always returns ``True``, so all the documents will be
displayed).

You get the following result: 

.. image:: images/m64d1e0e7.png

We just see a link :guilabel:`Go` which allows us to access the document we
have created. Now we need to add columns to this view.

Select ``Plomino: column`` from the :guilabel:`Add item` Plone menu.

Enter an identifier and a title, and select the field you want to display in 
the column.

.. image:: images/b38e0e1.png

You can also enter a :term:`formula` to compute the column value, for 
instance::

    return plominoDocument.getItem('bookTitle').upper()

.. Warning:: When you use a field as column value, the Plomino index will use
   the field index.
   So if you display this field as column in several views, it will not
   increase the index size.
   But when you create a formula, it will create a new column-specific
   index, so having a lot of column formulas might impact the database
   global performances.

Similarly, add a column to display ``bookAuthor``.

Columns can be ordered by going to the view's :guilabel:`Contents` tab and
moving the columns where needed.

If you go back to the Library database root, the view is proposed in the
:guilabel:`Browse` section: 

.. image:: images/m12df968f.png

Create more documents. When you click on the link :guilabel:`All the books`,
the view is displayed with its 2 columns (and its new documents): 

.. image:: images/6de65017.png

To improve browsing of the documents, it could be useful to sort the
view.

To do that, click on :guilabel:`Edit`, go to the :guilabel:`Sorting` tab and
enter ``col1`` in the :guilabel:`Sorting` column, then save: 

.. image:: images/193e0720.png


Add more views
==============

You can add as many views as necessary.

You can build views able to filter the documents; for instance if you
enter the following selection formula::

    return (plominoDocument.getItem('publicationYear') >= 1800 and 
        plominoDocument.getItem('publicationYear') < 1900)

you will only list the XIXth century books.

You can create *categorised* views: create a view with a first column
which contains the ``bookCategory`` field value, and select
:guilabel:`Categorised` in the :guilabel:`Sorting` tab: 

.. image:: images/m233a2bba.png

Each category can be expanded or collapsed. 

Dynamic view
============

Click on :guilabel:`Edit`, go to the :guilabel:`Parameters`, and change
widget to :guilabel:`Dynamic table`.  It renders the view using JQuery
Datatables (column sorting, live filtering, ...).

.. image:: images/dynamic-view.png

Add a search form
=================

Create a new form named ``frmSearch``, and add some fields with the same
identifiers as the documents fields you want to be able to search; for
instance: ``bookTitle``, ``bookAuthor`` and ``bookCategory``.

In the :guilabel:`Parameters` tab, select 'Search form' and enter ``all`` in
:guilabel:`Search view`: 

.. image:: images/22e7de63.png

This form is now proposed in the :guilabel:`Search` section in the Library
database root: 

.. image:: images/197da1a1.png

If you click on this link, you get the search form, and if you enter
some criteria, the results are displayed under the form: 

.. image:: images/m54d2b2e2.png

.. Note:: 
    the criteria are effective only if the field names match the
    document item names.


:guilabel:`About` and :guilabel:`Using` pages
==============================================

Go to the *Library* database :guilabel:`Edit` tab. You can fill in the 
:guilabel:`About this database` section and the :guilabel:`Using this
database` section.

Information entered here will be available under the :guilabel:`About` and
the :guilabel:`Using` tabs.
It allows you to offer users a page to describe the purpose of the
application and another one to give a short user guide.

