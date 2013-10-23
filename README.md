lcnc-buildbot
=============

notes and master.cfg for a buildbot which builds lcnc

-----
- Q:  A well-known buildbot already exists for the LinuxCNC (lcnc)
    project, managed and maintained by Sebastian Kusminsky and 
    publically accessible via http://buildbot.linuxcnc.org. So why
    is there this apparent duplication of effort?
- A1: I wanted to understand how buildbots work and what better than
    to create one for a project to which I occasionally contribute.
- A2: I wanted to build LCNC for branches, distros, and architectures not covered
    in the LinuxCNC buildbot.
- A3: There is strength in diversity.

-----

This repo contains my master.cfg for a buildmaster which controls a
assortment of builders running on an assortment of buildslaves which
were created using different distros and different architectures.

Passwords and personally identifiable information have been
sanitized.

Features of the resulting buildbot include

Version 1:

- every 5 minutes poll the LCNC repository 
  (git://git.linuxcnc.org/git/linuxcnc)
  for changes to the Unified Build Candidate 3 branch
- on a detected change or on receipt of a forced-build command,
  start the builders for the Unified Build Candidate 3 branch on
  all available buildslaves.
- report the results to a standard set of buildbot web pages
- enable specified users to force one or more builders to run on 
  available buildslaves and possibly do other things as well.  
- require at least 15 minutes to elapse after the last detected
  change before triggering a new build. This is an attempt to 
  minimize the following occurrance:
    - the first of a related set of commits to the repo triggers
      the buildmaster to start the builders
    - the builders are started sequentially
    - upon starting, each builder updates its private copy of the
      repo; depending on the timing of the commits and the starts, 
      the builders may not be updated to the same commit.
    - the results of the builds reflect different commits as 
      a result. 
