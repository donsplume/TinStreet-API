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

Printing: ``/printing``
-----------------------
GET
^^^
**Request (query string)**
    - ``name``: partial name of a card with which to search. Must be three or more characters.

**Response (body - list of objects)**
    - ``name``: the full card name;
    - ``code``: a unique printing code, a combination of card and expansion, and printing number where necessary;
    - ``expName``: the full expansion name;
    - ``printingNum``: the printing number of the card within the expansion;
    - ``image``: a full URL for an image of the printing.

Collection: ``/user/[username]/collection``
-------------------------------------------
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

GET
^^^
*Requires authorisation*

**Response (body - list of objects)**
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

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

