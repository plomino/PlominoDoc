=======================
Plomino class reference
=======================

Non-exhaustive list of the classes' methods.

.. todo:: TO BE UPDATED. Generate this from docstrings in the code?

PlominoDatabase
===============

.. note:: The includes methods inherited from base classes.

``callScriptMethod(self, scriptname, methodname, *args)``
    Calls a method named ``methodname`` in a file named ``scriptname``,
    stored in the ``resources`` folder. If the called method allows it, you
    may pass some arguments.

``createDocument(self)``
    Returns a new empty document.

``deleteDocument(self, doc)``
    Delete the document from database.

``deleteDocuments(self, ids=None, massive=True)``
    Batch delete documents from database. If ``massive`` is ``True``, the
    ``onDelete`` formula and index updating are not performed (use
    ``refreshDB`` to update).
    
``getAgent(self, agentid)``
    Return a PlominoAgent, or None.

``getAgents(self)``
    Returns all the PlominoAgent objects stored in the database.

``getAllDocuments(self)``
    Returns catalog brains for all the PlominoDocument objects stored in
    the database.

``getCurrentMember(self)``
    Returns the current Plone member.

``getCurrentUserRights(self)``
    Returns the current user's access rights.

``getCurrentUserRoles(self)``
    returns the current user's roles.

``getDocument(self, docid)``
    Returns the PlominoDocument object corresponding to the identifier, 
    or ``None``.

``getForm(self, formname)``
    returns the PlominoForm object corresponding to the identifier.

``getForms(self)``
    returns all the PlominoForm objects stored in the database.

``getIndex(self)``
    returns the PlominoIndex object.

``getPortalGroups(self)``
    returns the Plone site groups.

``getPortalMembers(self)``
    returns the Plone site members.

``getPortalMembersIds(self)``
    returns the Plone site member ids.

``getPortalMembersGroupsIds(self)``
    returns the Plone site groups ids and all the Plone site members
    ids.

``getUserRoles(self)``
    returns all the roles declared in the database.

``getUsersForRight(self, right)``
    returns the users declared in the ACL and having the given right.

``getUsersForRoles(self,role)``
    returns the users declared in the ACL and having the given role.

``getView(self, viewname)``
    return the PlominoView object corresponding to the identifier.

``getViews(self)``
    returns all the PlominoView objects stored in the database.

``hasUserRole(self, userid, role)``
    Returns ``True`` if the specified user id has the given role.

``isCurrentUserAuthor(self, doc)``
    returns ``True`` if the current user is author of the given document
    or has the PlominoAuthor right.

``refreshDB(self)``
    refresh the database index and the formulas.

``writeMessageOnPage(self, infoMsg, REQUEST, ifMsgEmpty = '', error = False)``
    displays a standard Plone status message.
    The REQUEST parameter is mandatory. Most of the time,
    ``plominoDocument.REQUEST`` will be the correct value. ``ifMsgEmpty`` is
    the default message to display if ``infoMsg`` is empty.  If ``error`` is
    ``False``, the message displays as an *informational* message; if
    ``True``, it displays as an *error* message.

.. _document:

PlominoDocument
===============

``delete(self, REQUEST=None)``
    deletes the document, and if ``REQUEST`` contains a key named
    ``returnurl``, uses its value to redirect the client.

``deleteAttachment(self,` `REQUEST)``
    remove file object and update corresponding item value.

``getfile(self, filename=None, REQUEST=None)``
    return the file corresponding to the given filename.

``getFilenames(self)``
    return the filenames of all the files stored with the document.

``getForm(self)``
    returns the form given by the *view form* formula (if the document
    is opened from a view and if the view has a form formula), else
    returns the form given by the document's Form item.

``getItem(self, name, default='')``
    returns the item value if it exists, else returns the default value (an 
    empty string if not provided).

``getItemClassname(self, name)``
    returns the class name of the item .

``getItems(self)``
    returns the names of all the items existing in the document.

``getParentDatabase(self)``
    Normally used via acquisition by Plomino formulas operating on
    documents, forms, etc.

``getRenderedItem(self, itemname, form=None, convertattachments=False)``
    returns the item value using the rendering corresponding to the
    field type defined in the form (if form is ``None``, it uses the form
    returned by ``getForm()``). If ``convertattachments`` is ``True``,
    FileAttachments items are converted to text (if possible).

``hasItem(self,` `name)``
    returns ``True`` if the item exists in the document.

``isAuthor(self)``
    returns ``True`` if the current user is author of the document or has
    the PlominoAuthor right.

``isEditMode(self)``
    returns ``True`` is the document is being edited, ``False`` if it is
    being read. Note the same method is available in PlominoForm, so it
    can be used transparently in any formula to know if the document is
    being edited or not.

``isNewDocument(self)``
    returns ``False`` (because an existing document is necessarily not
    new). Note the same method is available in PlominoForm (and returns
    ``True``), so it can be used transparently in any formula to know if
    the document is being created or not.

``openWithForm(self,` `form,` `editmode=False)``
    display the document using the given form's layout (but first, check
    if the user has proper access rights).

``removeItem(self,` `name)``
    remove the item.

``save(self, form=None, creation=False, refresh_index=True)``
    refresh the computed fields and re-index the document in the Plomino
    index and in the Plone ``portal_catalog`` (only if ``refresh_index`` is
    ``True``; ``False`` might be useful to improve the performance, but a
    ``refreshDatabase`` will be needed). It uses the field's formulas
    defined in the provided form (by default, it uses the form returned
    by ``getForm()``).

``send(self, recipients, title, form=None)``
    send the document by mail to the recipients. The document is
    rendered in HTML using the provided form (by default it uses the
    form returned by ``getForm()``).

``setItem(self,name,value)``
    set the value (if the item does not exist, it is created).

.. _form:

PlominoForm
===========

``getFormName(self)``
    returns the form id.

``getParentDatabase(self)``
    returns the PlominoDatabase object which contains the form.

``isEditMode(self)``
    returns ``True``. 
    
    .. Note:: 
        the same method is available in PlominoDocument, so it can be
        used transparently in any formula to know if the document is
        being edit or not.

``isNewDocument(self)``
    returns ``True`` (when the context is a form, it is necessarily a new
    doc). 
    
    .. Note:: 
        the same method is available in PlominoDocument (and returns
        `False`), so it can be used transparently in any formula to know
        if the document is being created or not.

.. _view:

PlominoView
===========

``exportCSV(self, REQUEST=None)``
    returns the columns values in CSV format. If REQUEST is not ``None``,
    download is proposed to the user.

``getAllDocuments(self)``
    returns all the documents which match the Selection Formula.
    Documents are sorted according the sort column (if defined).

``getDocumentsByKey(self, key)``
    returns all documents for which the value of the column used as sort
    key matches the given key.

``getParentDatabase(self)``
    returns the PlominoDatabase object which contains the view.

``getViewName(self)``
    returns the view id.

PlominoIndex
============

``dbsearch(self, request, sortindex, reverse=0)``
    searches the documents corresponding to the request (see ZCatalog
    reference). The returned objects are ZCatalog brains pointing to the
    documents (see ZCatalog reference).

``getKeyUniqueValues(self,` `key)``
    returns the list of distinct values for an indexed field.

``getParentDatabase(self)``
    returns the PlominoDatabase object which contains the index.

``refresh(self)``
    refresh the index.

PlominoUtils
============

.. Note::
    PlominoUtils is imported for any formula execution, its methods are
    always available (importing the module is not needed).

    Another module with some useful methods is
    ``Products.PythonScripts.standard``, which can be imported if needed.

``actual_context(context, search="PlominoDocument")``
    return the actual context from the request, it will drill into the 
    path until it find a context matching the searched class.
    Useful in portlet context

``actual_path(context)``
    return the actual path from the request.
    Useful in portlet context

``array_to_csv(array, delimiter='\t', quotechar='"')``
    Convert ``array`` (a list of lists) to a CSV string.

``asList(x)``
    If not list, return x in a single-element list.
    .. note:: If ``x`` is ``None``, this will return ``[None]``.

``asUnicode(s)``
    Make sure ``s`` is unicode, decode according to site encoding if needed.

``csv_to_array(csvcontent, delimiter='\t', quotechar='"')``
    Convert CSV to array. ``csvcontent`` may be a string or a file.

``DateRange(d1, d2)``
    returns the dates of all the days between the 2 dates.

``DateToString(d, format=None, db=None)``
    Converts a date to a string. If the database object is passed for the
    ``db`` parameter, use the database date format.

``htmlencode(s)``
    Replaces unicode characters with their corresponding HTML entities.

``isDocument(object)``
    Test if the object is a ``PlominoDocument``.
    Useful to distinguish a document context from a form context.

``json_dumps(obj)``
    Return the object as a string using the JSON format. Example::

        >>> json_dumps({"a": [1, 2, "This is a 'quote'"], "b": 0.098098})
        '{"a": [1, 2, "This is a \'quote\'"], "b": 0.098098}'

``json_loads(s)``
    Build an object from a JSON string. Example::

        >>> json_loads('{"a": [1, 2, "This is a \'quote\'"], "b": 0.098098}')
        {u'a': [1, 2, u"This is a 'quote'"], u'b': 0.098098}

``Log(message, summary='', severity='info', exc_info=False)``
    Write a message to the server event log.

``Now()``
    returns current date and time as a DateTime object.

``open_url(url, asFile=False)``
    Retrieve content from ``url``

``PlominoTranslate(message, context, domain='CMFPlomino')``
    translate the given message using the Plone i18n engine (using the
    given domain).

``sendMail(db, recipients, title, html_message, sender=None, cc=None, bcc=None, immediate=False)``
    Send a mail to the recipients.
    If sender is None, it will use the current user mail address.

``StringToDate(str_d, format='%Y-%m-%d', db=None)``
    Converts a string to a date.  If ``db`` is passed, use the database date
    format.  If ``format=None``, guess. 

``PlominoTranslate(msgid, context, domain='CMFPlomino')``
    Look up the translation for ``msgid`` in the current language.

``urlencode(h)``
    Convert a dictionary into a URL querystring (a ``key=value&`` string).
    Example::
    
        >>> urlencode({"option": 5, "article": "9879879"})
        'article=9879879&option=5'

``urlquote(string)``
    Replace special characters in a string using the ``%xx`` escape.
    Example::
    
        >>> urlquote('runAgent?REDIRECT=True&action=accept')
        'runAgent%3FREDIRECT%3DTrue%26action%3Daccept'

``userFullname(db, userid)``
    returns the user full name.

``userInfo(db, userid)``
    returns the Member object corresponding to the user id (it may be
    used to get the user email address for instance).

.. _agent:

PlominoAgent
============

``getParentDatabase(self)``
    returns the PlominoDatabase object which contains the agent.

``runAgent(self, REQUEST=None)``
    runs the agent. If REQUEST is provided, there is a redirection to
    the database home page, unless the REQUEST contains a REDIRECT key
    If so, the formula returned value is used as the redirection URL.

``__call__(*args)``
    if agents are called from Python code, they can take positional
    arguments.

