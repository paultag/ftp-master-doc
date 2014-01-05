Unchecked
=========

Unchecked is the job that does a lot of heavy lifting on ftp-master. It's the
job that runs most frequently, running 4 times an hour. At the time of this
writing, unchecked runs every hour at ``:02``, ``:17``, ``:32``, ``:47``.

Unchecked does a lot of important stuff, most importantly, processing the
policy queues, and some light housekeeping.


Procedure
---------

The following is a quick digest of what unchecked does (in order)

  - process policy queues (stable-new, oldstable-new, backports)
  - clean suites packports-policy, policy
  - do unchecked processing
    - (find all changes)
    - process-upload
    - process-commands
  - sync debbugs
  - do some buildd stuff
    - make-overrides
    - make_buildd_dir
      - manage-build-queues
      - generate-packages-sources2
      - generate-releases
      - update-buildd-archive
      - rm public/*
      - export-suite -s "accepted" -d "$incoming/public"
    - wbtrigger
      - wbadm@buildd /org/wanna-build/trigger.often
  - dak contents
    - scan-binary
    - scan-source
