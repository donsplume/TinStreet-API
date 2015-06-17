.. TinStreet documentation master file, created by
   sphinx-quickstart on Tue Nov 25 13:39:44 2014.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to TinStreet's documentation!
=====================================

Contents:

.. toctree::
   :maxdepth: 2

Printing
--------
Make a ``GET`` request to ``printing?name=[name]``, where ``[name]`` is a search string for a partial card name. Results are only returned if ``[name]`` is three or more characters. Results will be a JSON list of objects with the following attributes:

- ``name``: the full card name;
- ``code``: a unique printing code, a combination of card and expansion, and printing number where necessary;
- ``expName``: the full expansion name;
- ``printingNum``: the printing number of the card within the expansion;
- ``image``: a full URL for an image of the printing.

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

