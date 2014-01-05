Monthly cron run
================

The monthly cron run takes place on the 1st day of the month, at ``06:00``.
This mostly deals with log rotations.


Procedure
---------

  - rotate mail and bxamail
  - mark the current logs as the current ones
  - rotate queued logs out of the way
