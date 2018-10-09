.. _agenda_tor_mx:

Bandwidth authority session, September 2018
=============================================

Desired attendees
------------------

- dirauths
- net-team
- test-net
- metrics-team

Status
--------

past meeting goal: :ref:`requirements`

- something that makes measurements:
  paths, file size, destination, etc independent and configurable
- easy to understand, maintain and run:

  - proper python package, minimal requirements
  - docs: extensive docs
  - system package: Debian, OpenBSD Makefile

DONE! :-)

briefly, how sbws works
------------------------

- net

  - 2 hops
  - prioritization entry
  - web server
- aggregation

  - scaling

Details: :ref:`sbws`

Bandwidth values in Tor
------------------------

:ref:`bandwidth_tor`

Torflow aggregation
--------------------

:ref:`torflow_aggr`

Mostly depends on descriptor ``observed-bandwidth``, ie relays self-reported
bandwidth.

How to measure improvements
-----------------------------

:ref:`measure_improvements`

Infrastructure
---------------

- where scanners should be?
  - 1 in Germany and 1 in USA cause it is where most of the bandwidth is?
  - possible providers: better coop, no big companies? (TODO: example providers)
- who should run them?
  - volunteers
- how they should be funded?
  - torservers and similar
- where the web servers should be?
  - 1 in CDN: how to get that in Torproject fastly?
- should they be in the same locations as the scanners?

- how secure needs the machine to be?
- do they need to be a dedicated server?
- is it important the physical access of the server?


Others
--------

- documentation
- work organization

Notes; [BwAuthMex]_