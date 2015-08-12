.. TinStreet documentation master file, created by
   sphinx-quickstart on Tue Nov 25 13:39:44 2014.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to TinStreet's documentation!
=====================================

Contents:

.. toctree::
   :maxdepth: 2

All POST requests that require a request body are assumed to have been JSON string serialised.

All API response bodies will be JSON strings, either representing objects, or lists of objects.

Login: ``/login``
-----------------
A resource representing an authorised user's 'session' with the API. After estabilishing a session, a user can access and modify certain resources that an unauthorised user cannot, for example user-specific private information such as their email address, or market orders.

POST
^^^^
Used to create a new user login session. If the user is successfully authenticated, a cookies will be returned that can be used in future requests to resources that require authentication.

The request is made with the user's credentials, which are verified and the API returns either an error or a description of a new login session, along with associated cookies.

It is assumed that the session cookies, once created, and sent with every API request, and will be used by the server to firstly authenticate the user (where required) and secondly to determine authorisation for interacting with the specified resource with the chosen method.

**Request (body)**
    - ``username``: the username of the user for whom to create the session;
    - ``password``: the corresponding password of the user in plaintext.

**Response (body)**
    - ``user``: username of logged in user;
    - ``sessionId``: unique ID representing the current login session;
    - ``expiry``: the time at which the session will be unavailable, in the 'cookie' format (see `RFC 2109 <http://tools.ietf.org/html/rfc2109.html>`_). Each request to the API by an authorised user whilst the session is active will push back the session expiry, keeping it an hour in the future.

**Response (cookies)**:

    - ``user``: username of logged in user;
    - ``sid``: unique ID representing the current login session.

    The cookies are marked as 'HTTP only' and to expire after one hour of inactivity.

GET
^^^
*Requires authentication*

Used to request the details of the authenticated user's login session. Client applications may not be able to access the contents of the session cookies (e.g. JavaScript cannot access 'HTTP only' cookies, and some browsers will set all cookies returned by a server as 'HTTP only', regardless of what was specified on the response), so this method can restate the session details in the response body.

**Response**
    - ``user``: username of logged in user;
    - ``sessionId``: unique ID representing the current login session;
    - ``expiry``: the time at which the session will be unavailable.

Instrument
----------
 ``/instrument``

GET
^^^
**Request (query string)**
    - ``name`` *string*: partial name of a card with which to search. Must be three or more characters.

**Response (body - list of objects)**
    - ``instrumentCode`` *string*: a unique instrument code, a combination of printing and condition;
    - ``condition`` *string*: the condition code for the instrument;
    - ``printingCode`` *string*: the code for the instrument's printing. A combination of card, expansion, and foil and printing number where necessary;
    - ``expCode`` *string*: the code for the printing's expansion;
    - ``expName`` *string*: the full expansion name of the printing;
    - ``foil`` *integer*: whether the printing is foil (1) or not (0);
    - ``printingNum`` *string*: the printing number of the card within the expansion;
    - ``image`` *string*: a full URL for an image of the printing;
    - ``cardCode`` *string*: the code for the printing's underlying card;
    - ``name`` *string*: the full card name.

Collection
----------
``/user/[username]/collection``

POST
^^^^
*Requires authorisation*

Used to create a new Collection Item in the specified user's Collection.

**Request (body)**
    - ``card``;
    - ``printing`` *optional*;
    - ``instrument`` *optional*;
    - ``expCode`` *optional*;
    - ``foil`` *optional*;
    - ``condition`` *optional*;
    - ``privatePosition`` *conditional*;
    - ``hostedPosition`` *conditional*.

    Either ``privatePosition`` or ``hostedPosition`` must be set.

**Response (body)**
    - ``itemID``: unique ID for the Collection Item.

GET
^^^
*Requires authorisation*

**Response (body - list of objects)**
    - ``itemID``;
    - ``name``;
    - ``card``;
    - ``printing``;
    - ``instrument``;
    - ``expName``;
    - ``expCode``;
    - ``foil``;
    - ``condition``;
    - ``privatePosition``;
    - ``hostedPosition``.

Collection Item
---------------
``/user/[username]/collection/[itemID]``

PUT
^^^
*Requires authorisation*

Used to update the specified Item.

**Request (body)**
    - ``card`` *string*: the card code;
    - ``printing`` *string, optional*: the printing code;
    - ``instrument`` *string, optional*: the instrument code;
    - ``expCode`` *string, optional*: the expansion code;
    - ``foil`` *integer, optional*: whether the Item represents a foil printing (1) or not (0);
    - ``condition`` *string, optional*: the condition code;
    - ``privatePosition`` *integer, conditional (note 1)*;
    - ``hostedPosition`` *integer, conditional (note 1)*.

(1) Either ``privatePosition`` or ``hostedPosition`` must be set.

Each field can be set to the existing or updated value. Note, any unset fields will be treated as a null value.

**Validation and Processing**

If the instrument code is supplied, a lookup of condition, printing, foil, card and expansion is performed and compared against any supplied values for ``condition``, ``printing``, ``foil``, ``card`` or ``expCode`` respectively - any inconsistencies will generate an error. The same applies if instead the printing code is supplied, but for ``foil``, ``card`` and ``expCode`` only.

DELETE
^^^^^^
*Requires authorisation*

No request body required, no response body returned.

Album
-----
``/user/[username]/album`` (for POST)

An album is similar to a Collection, but a user may have multiple albums and does not need to hold a position in any of the items within an album.

POST
^^^^
*Requires authorisation*

Used to create a new album for the given user.

**Request (body)**
    - ``name`` *string*: the name for the new album, which must be unique for the user.

**Response (body)**
    - ``name`` *string*: the new album's name;
    - ``albumId`` *string*: a unique ID for the album, to be used in subsequent requests.

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

