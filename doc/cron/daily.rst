Daily cron run
==============

The daily run takes place every day at ``03:09``. This handles some things
that might as well be done daily, mostly related to pestering the ftpteam.


Procedure
---------

  - Nag the ftpteam about NEW/BYHAND packages.
  - Generate the cruft report and nag the ftpteam
  - Clean debbugs in queuedir
    - (delete any sync'd files older than 60 days)
    - (delete any empty dirs)
  - Generate override disparities list
  - Generate NEW queue stats, write to NEW-stats.yaml
  - link_morgue.sh
