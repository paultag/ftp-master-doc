Dinstall
========

The dinstall cron run should be done at ``1:52``, ``7:52``, ``13:52``,
and ``19:52``. dinstall does some of the most important stuff on ftp-master.
it's also the most long-running, and complex.

Procedure
---------

  - qa1 (tell qa we're starting ``dinstall``)
  - updates( ``update-{bugdoctxt,mirrorlists,mailingliststxt,pseudopackages.sh``})
  - i18n1 (``rsync`` package i18n descriptions)
  - do automated ``p-u-new`` for ``stable`` and ``oldstable``
  - do automated backports ``p-u-new`` for ``stable`` and ``oldstable``
  - ``dak check-overrides``
  - ``dak dominate``
  - ``dak generate-filelist``
  - ``dak import-keyring``
    - import the ``keyring.d.o`` keyring (``dak import-keyring -L``)
    - If there's output to ``dak import-keyring --generate-users "%s"`` on the
      keyring from ``keyring.d.o``, send mail to the ``debian-project`` list
  - do overrides
    - ``dak make-overrides``
    - cal all sections into ``override.sid.{section}``, and append to ``.all3``
  - make package file mapping
    - ``dak make-pkg-file-mapping {archive}`` gets written to ``package-file.map.bz2``
  - generate pdiffs (``dak generate-index-diffs``)
  - gitpdiff, add pdiffs to a git repo, commit it and tag it.
  - generate release files
    - for each public archive,
      - ``dak generate-releases -a {archive}``
  - clean up old packages and files
    - ``dak clean-suites -m 10000``
    - ``dak clean-queues -i uncheked``
  - create maintainer index
    - for each public archive:
      - dak make-maintainers -a {archive}
  - copy overrides (for each suite and section)
  - make files indicies
    - generate a sources list
    - generate an arch list
    - generate a suite list
    - update ``sundries.txt``
    - generate files list
  - make checksums
    - run ``dsync-flist`` a bunch (``-q {generate,md5sums,link-dups}``)
  - regenerate ``mirror/`` hardlinks, do some ``rsync``
  - sync with the dd accessable copy
  - do some changelogs
    - ``dak make-changelog -e -a ftp-master``
    - ``rsync`` them
    - ``staticsync``
    - ``dak make-changelog -e -a backports``
    - ``rsync`` them
  - expire_dumps
  - clean transitions
    - ``dak transitions -c -a``
  - update dm rights page
    - ``dak acl export-per-source dm >$exportdir/dm.txt``
  - Categorize uncategorized bugs filed against ``ftp.debian.org`` in the bts.
    - ``dak bts-categorize``
  - Do the mirror push (``/home/archvsync/runmirrors``)
  - Do the backports mirror push (``/home/backports/bin/update-archive``)
  - Export data for i18n, sign it
  - Update some stats
    - generate ``ftpstats.data``
    - ``dak stats arch-space``
    - ``dak stats pkg-nums``
  - Update testing.list (``dak ls -s testing -f heidi -r .``)
  - Clean any transaction older than 3 months
  - Rename the log file to dinstall_${NOW}
