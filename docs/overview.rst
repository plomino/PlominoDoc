==================================
Background and highlevel overview
==================================

Plomino is an interesting extension of Plone. 

In a time when Plone is narrowing its focus on document-based CMS, Plomino
offers in-Plone application-building. 

In a time when through-the-web, :term:`ZODB`-resident scripting has fallen
out of favour, Plomino does everything through the web. 

It has an interesting ancestry too: the name and some core ideas hark back
to Lotus Domino (https://en.wikipedia.org/wiki/IBM_Lotus_Domino), which
besides a lot of other things was a form-based application builder backed by
an object database, from the early 90's. As such, in some respects it's a
conceptual forerunner of Zope.

Plomino trades a number of core Plone concepts for flexibility and
simplicity. In the first place, it offers only one type of content: a
generic :ref:`Document <document>` type. Secondly, it eschews containment
for documents, using Plone's containment system only for its own simple
application structure.  Similarly, Plone's cut/copy/paste operations don't
make sense for Plomino documents.

A Plomino application or database is a single container which holds
:ref:`Forms <form>`, :ref:`Views <view>`, and :ref:`Agents <agent>`. It also
has a catalog, a ``documents`` container for all documents, and a
``resources`` container for script libraries. That's it. 

A closer look 
==============

*Forms* contain *Fields* and *Actions*, and *Views* contain *Columns* and
*Actions*. 

When you start to build a Plomino application, you normally start by adding
Forms. When you add a form, the creation page resembles a normal Plone Page,
with a large richtext edit field. Here, you simply type out the layout of
the form. Add tables, images, explanatory text, whatever you need. The 
one departure: you also create *fields*, *actions*, *hide-whens*,
*subforms*, *accordions*, and *cache-zones* on the layout using
Plomino-specific tinymce buttons. 

This allows very quick prototyping of forms, and it broadens participation
in form design. This is not equally valuable for all applications, or for
all stages of an application's lifecycle, but it can be very useful. 

As you add fields and so on, :doc:`Field <fields>` and :ref:`Action
<actions>` instances are added to the Form. These are matched with layout
elements according to id: the ids match elements on the layout, and the
widget of the field or action is substituted for the placeholder on the
layout when the form is viewed.

When viewing the form, a user can fill in the field widgets and submit the
form using the default :guilabel:`Save` action. At this point, a
:term:`Document` is created containing :term:`Item`\s that correspond to the
:term:`Field`\s on the :term:`Form`. 

Follow me closely here: if you create a *Book* **form** with *Title* and
*Author* **fields**, this will create **documents** with *Title* and
*Author* **items**.  Another form, say *Trip*, may create documents with
items like *Departure*, *Destination* and *Passenger*. Documents are simply
generic bags of items.  They are both created and viewed using forms, that
render the items found on the document using fields corresponding fields on
the form. (Note that a given form may not have fields for all the items on
the document, and there may be fields that do not correspond to items but
that render values based on other items or other documents.)

With Plomino, you have to build the additional structures you need using
documents and items as building blocks.

While creating documents, it may be useful to think of a Form in terms of a
rubber stamp. When you use it to create a document, it stamps its items on
that document, at that moment. If you change the Form afterwards, the items
on the documents created previously will still be the same: you may need to
re-save documents with the latest version of the form if you need their
items to be updated. 

While viewing documents, you are also using forms. At this point it's more
useful to think of a Form in terms of a template or mask: the form will
render the items that correspond to its fields.

When you use a Form to create or edit a document, it stores its name in a
``Form`` item on the document, so you could grab all books by looking for
the documents where the ``Form`` item is ``Book``.  However, Plomino doesn't
require that you always use the ``Book`` form for editing those documents.
If you added a ``CatalogBook`` form with fields like ``Dewey`` and ``ISBN
number``, for the use of users doing cataloging, and go over the book
documents using this form, their ``Form`` items will change to
``CatalogBook``.  Therefore one common pattern is to include a ``doctype``
field on forms used to create documents (if, indeed, your Plomino
application requires the concept of different types of documents). 

Similarly you could include an item referencing a ``parent`` document if you
wanted to mimic containment, but this is only one possible way of
structuring your data.

Grouping documents
===================

Forms are built around individual Documents. For dealing with Documents in
aggregate, Plomino offers :ref:`Views <view>`. The documents in a view are
*all the documents for which the selection formula (Python Script) on the
View evaluates as ``True``*. Views contain :ref:`Columns <column>`, that are
calculated for each matching document. They often correspond to items on
documents, but can be any value returned by a formula. That is, each record
in a view corresponds to a Document, but the values of columns in the record
need not come from that Document.

Views are updated as documents are created or edited, but depending on the
formula and the number of documents, views can be expensive to refresh from
scratch.

Besides grouping documents, views are also useful for browsing purposes.
They allow paging and filtering, and can evaluate a formula to determine
which Form should be used for viewing documents opened from the view (that
is, a view that lists books for lending could show documents using a
*Checkout* form, while a view that lists books with incomplete metadata
could use the *CatalogBook* form).

Security 
==========

- All the normal Plone roles and permissions pertain to Plomino. 
- In addition, Plomino offers a hierarchy of roles that govern management of
  the application, creation and editing of Forms and other design aspects,
  creating and editing documents using the supplied forms, and accessing the
  database. 
- Finally, Plomino allows creation of user-defined roles that can be
  assigned to Plone principals, and need to be checked for at
  application-level in the Plomino application.

As such, security is to some extent leaky, depending on application authors
to remember the appropriate checks in all relevant forms.  Also, the form to
be used for rendering a document can be passed as an URL parameter, so 
someone could sneak a look at a document using a form that you didn't 
intend, as can form values, and various other API games.  This can be
mitigated by factoring out certain checks to a common script library and
including them in all forms, but I think you get the point --- Plomino does
not chase the grail of a bulletproof environment.  You need to think about
what is *enough* security, and not deploy Plomino applications with data
inappropriate to the context (i.e. deploy applications with sensitive data
to closed groups).

Barely-repeatable processes, workflow 
=====================================

There are countless cases of people, businesses or projects switching bug
tracking systems to find one that fits their way of working. And a bug
tracking system is a relatively simple domain! Most processes are much more
complicated. Does this really make sense? A bug tracking system includes
implementation choices and policies regarding database backend, templating
mechanism, authentication sources, and so forth and so on, in addition to
the business rules of bug tracking. It's a shame that everything else has to
change if you all you really want to change are the business rules.

Any application deployed in a real-world environment ends up having to deal
with local variations, transient changes, emerging requirements, and having
the business change in response to the application being implemented.

Of the various ways in which to confront this reality, one method is to use
an architecture that provides simple building blocks. The architecture can
remain stable across deployments and evolve in a controlled fashion, while
the various deployments of the application can be tweaked in place,
branching and diverging if needed. 

This is especially true for Plomino, which is meant for quickly creating 
solutions where exhaustively analysing and modeling the domain is not
justified; or indeed, where a Plomino solution is instrumental in building
up the business knowledge necessary to realistically model a good solution,
while incidentally getting work done.

This is a powerful motivation of the "dirty" mixing of content and code 
in the database.

Workflow 
==========

One way of addressing workflow needs in Plomino is to create a script
library which computes the form which should be used based on the context
(what is being viewed by whom). However Plomino itself doesn't offer
building blocks to make building workflows easy and consistent. 

This makes associating security with workflow states more arduous than
ideal.

Use cases 
=========

Use cases: 
- simple form-based data capture.
- mini-apps that manipulate Plone content.
- selfcontained apps.
- replicate forms/data to other instances.
- pull/integrate data from other sources.

Plomino has different sweet spots. One of the quickest is simple form-based 
data capture. On this level, it is PloneFormGen_'s more free-spirited cousin.

.. _PloneFormGen: http://plone.org/products/ploneformgen

It can also be used to manipulate Plone content, similarly to 
:term:`Content Rules`, but again, it's easier to script case-by-case
variations from Plomino than using Rules. This is a good case for Plomino
micro-apps consisting only of a couple of forms with some scripts to drive
Plone, e.g. pre-populating an event folder with Event, NewsItem, and PR
announcements.

Once the bug has bitten, it's also very tempting to build entire
self-contained applications in Plomino. In some cases this makes sense (for 
example, Plomino data and applications can be synced between Plone
instances, so if you need (parts of) your application to be synced, it has
to stay in Plomino), but the goal should always be to build as little as
possible. For example, it would be a pity to build a bug tracker in Plomino.

Regarding the replication use cases: imagine a library environment. The 
forms for browsing books are synced to the public servers, but the forms 
for editing the catalog are kept on the librarians' servers. Or imagine a
business with different branches. The data from each branch is synced to the
head office to be aggregated, and pricelists are synced to branches.

Plomino can also function as a very easy integration point with legacy or
third-party systems. Just arrange to push CSV to the URL of a Plomino view,
or for another service to pull CSV from a Plomino view (or form or agent,
depending on your needs), and complete the integration using Plomino Forms. 

Digging deeper 
================

Plomino looks nice and simple at first glance, but it allows you to get
yourself into as much trouble as you like ;-)

It is conceptually quite simple, and applications are fully defined by the
XML export.  The core Plomino concepts could be re-implemented on Dexterity
or Pyramid or Django without too much trouble.  Living in a CMS has its
advantages, however. The Zope and Plone APIs make a lot of power available.

Building pages 
---------------

It is easy to think of Plomino in terms of simple forms-based data capture.
However forms can have conditional sections, and can contain sub-forms.  In
addition, fields can return the rendered HTML of other forms; for example,
in the ``Milestones`` field on a ``Project`` document you could look up and
iterate over all the associated ``Milestone`` documents, get each one to
render itself using an appropriate form, and include the HTML in the
``Project`` view. You could even return arbitrary javascript to be executed
upon rendering of a form. So though you can write forms simply as richtext
documents, you are also free to compute any HTML you need. For this, you
have a number of mechanisms: render documents using forms or fields,
override the template used for fields or views with a template of your own,
or compute exactly what you need in Python. 

It is a matter of judgment at which point this becomes unmanageable. It can 
allow a quicker turnaround than a Python-product-based approach, but without
discipline it can result in a hard-to-understand mess. 

Application export and versioning 
----------------------------------

Some of the drawbacks of old-style through-the-web coding in Zope include:
- it's hard to distinguish between application and data;
- it's easy to lose track of application elements among nested folders with
  acquisition in play;
- it's hard to version the application. 

These are mitigated in Plomino in various ways:
- A Plomino application consists of a single container with design elements
  (forms, views, agents), and a ``resources`` subfolder with scripts,
  templates, images, and other collateral.
- The application can be exported to XML files. The ordering and formatting
  of the XML is consistent and can be usefully versioned. The XML files can
  be imported to update an instance to a particular version of an
  application.

Data migration 
---------------

As mentioned before, forms and documents are not tightly coupled. It's quite
easy to end up with a mix of documents from the time before books had a
``Translator`` item and later documents that do have that item and others.

In order to deal with this, sometimes all that is needed is to code
defensively. Instead of assuming that all documents will have a
``Translator`` item, show a default value if they don't. However if it is
necessary for the item to exist, the documents need to be updated. Various
approaches are possible: in the simplest case, just call the 
`save() <document>`_ method on all documents. In more complicated scenarios,
documents may need to be saved using specific forms or by a user with a
specific role. This can be dealt with by creating a Plomino :term:`Agent`
which does the required migration.

Once there are a lot of documents, re-saving all necessary documents can
take a long time. For this reason, as with all long-running Zope tasks, it's
best to kick off the migration on a ZEO client set aside for jobs like this.

Caveats 
--------

A quick list of ways to make life difficult for yourself:
- Change the field type after you already have documents with items of the
  original type (e.g. you used to be creating strings, but now you're
  creating dates).
- Store complex values as items (like arrays with inconsistent formats
  including CSV strings).
- Store derived fields that are not computed for display (once you do this,
  you have to worry about keeping derived fields current when editing the
  reference documents).
- Have a field called "A" in both forms "B" & "C", both used to show doc
  "D", but the definition of the field on "B" is incompatible with the field
  on "C". (This could happen if you forget to update both forms and migrate 
  existing documents.)

Ideas for improvement 
=======================

Plomino has been conservative, preferring to remain open-ended and
conceptually simple. While it could be made more sophisticated in many
ways, it's easy to lose some good properties in the process, such as the
ability to export and version the application in its entirety, or to easily
sync design elements and documents among Plomino instances.

Functionality 
--------------

That said, the current weak areas of Plomino are security, workflow, and
references, as they must be implemented manually using formulas.

Regarding workflow, perhaps AlphaFlow could be resurrected, or `zope.wfmc`_
or `hurry.workflow`_ could be used. A DCWorkflow-based approach would not
work, as all Plomino documents share the same type, and live in the same
folder.

.. _zope.wfmc: http://pypi.python.org/pypi/zope.wfmc
.. _hurry.workflow: http://pypi.python.org/pypi/hurry.workflow

Currently references between documents in Plomino tend to be simplistic,
consisting of storing document paths or ids as items. This makes transitive 
relationships or keeping track of constraints on relationships error-prone
and cumbersome. On the other hand, it is robust in its simplicity. If a
reference engine such as `zc.relationship`_ were used, there would be the
potential for the documents to get out of sync with the relationship index
due to import or sync operations.

.. _zc.relationship: http://pypi.python.org/pypi/zc.relationship

Another wrinkle regarding relations is that Plomino documents are identified
by their id, which should normally not change. By default, the id is a
random key. It is possible to compute something more readable, but be
careful of doing so prematurely, as it makes you worry about id collisions
and the continued suitability of ids chosen at the outset. Since Plomino
documents can be synced among Plomino applications, relations cannot depend
on object identity.

Performance 
------------

It's easy to make a big Plomino database crawl. The code being executed is
Restricted Python, and rendering a form which pulls content from many
related documents can pull lots of big fat Archetypes-based objects into
memory.  The contents of a view is anything that evaluates ``True`` for the
view's selection formula, which may be expensive. Not bad when done
incrementally, but can be pretty bad when refreshing the view for thousands
of documents.

Plomino does provide an extension mechanism, so you can move aspects of your
application to filesystem-based Python code if they are mature enough and
prove to be bottlenecks. 

