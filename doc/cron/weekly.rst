Weekly
======

The weekly run takes place on the 0th day of the week, at ``00:12``. This
does some stuff that doesn't really need to be run daily, but more often
than monthly would be nice.

Procedure
---------

  - prune empty directories in the pool
  - do a ``git gc --prune`` to ``dak.git``, ``git update-server-info``
  - fix the symlinks in ftpdir
