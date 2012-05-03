==========
 Glossary
==========

This is a glossary for Plomino-specific terms.  It is still heavily under
construction.

.. glossary:: :sorted:

    Agent
        An Agent object executes some code as a configured user.

    Field
        A Field object, which lives in a Form, renders a Widget, sometimes
        accepts input, and triggers events.

    Form
        A Form object, containing fields, edits :term:`Document` objects 
        according to the fields and events configured on the form.

    Sub-form
        A Form object rendered as part of another form.

    Document
        A Document in Plomino contains named :term:`Item` objects, that are 
        rendered by :term:`View` and :term:`Form` objects.

    Item
        An Item stored a value on a :term:`Document`.

    Column
        A Column defines a value that should be computed for each
        :term:`Document` that matches a :term:`View`.

    View
        A View defines a group of Documents related by a *selection
        formula*.

    Content Rules
        A Plone feature allowing code or events to be configures upon 
        events such as object creation or modification.

    Formula
        In the context of Plomino, a *formula* is a snippet of restricted
        Python code executed in the context of a field or event.

    Hide-when
        A :term:`formula` that determines whether an action or section of a
        form is rendered.

    Action
        An Action in Plomino renders a UI element that allows the user to 
        trigger some processing.

    ZODB
        The Zope Object Database: transparent persistence for Python
        objects, since 2002. (Before 2002, it was part of Zope.)

    ZMI
        The Zope Management Interface.
