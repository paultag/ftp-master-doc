Hourly cron run
===============

The Hourly cron run should be done at the top of every hour (every hour once
at ``:00``. This does some slightly less expensive stuff then dinstall does,
and semi-important enough to be done every hour.


Procedure
---------

The following is a quick digest of what ``cron.hourly`` does (in order)

  - import postgres users from ``/etc/passwd``
  - do some queue-report (e.g. ``new.html`` and friends)
  - do show-deferred (write to ``deferred.html``)
  - generate the graphs in web/stat
  - write out ``removals-full.txt`` and ``removals-full.822``
  - update queue rss
  - update removals rss
  - tell dd copy to sync it's tree
  - generate-di
  - do backports-master acl changes, email the team about it
  - update buildd (``buildd-{remove-keys,add-keys,prepare-dir}``)
  - for each keyring in ``dak admin k list-binary``, import the keyring and
    generate users.
