Torflow in detail
==================

- `Torflow code <https://gitweb.torproject.org/torflow.git>`_,
- `Torflow paper <https://research.torproject.org/techreports/torflow-2009-08-07.pdf>`_,
- specs: in the code repo,
- `wiki <https://trac.torproject.org/projects/tor/wiki/doc/BandwidthAuthority>`_

- what torflow is doing, in a broad sense:
    | * launch 2 tor clients
    | * repeat as often as possible, running 9 different scanners:
    |     * split relays into buckets by bandwidth percentile
    |     * build two hop paths with a relay and exit from relays in the bucket
    |     * download a file from a bandwidth server, choose the size based on the bucket
    |     * measure how long it takes
    |     * store the results in a database
    | * aggregate the results hourly:
    |     * produce a consensus weight to advertised bandwidth ratio
    |     * using a decaying weighted average
    |     * and some form of feedback (PID) control
    |     * and dump it to a file
    | Then authorities read this file and include it in their votes.

[#]_

Algorithm to build two hops path
---------------------------------------

This needs to be reviewed:

- take all relays, order by bandwith
- divide in consecutive slices of 50 relays
- for each relay in slice:
  - take a (random?) exit in the slice (from the set of exits in that slice minus the relay itself)
  - build a path with the relay and the exit
  - download a file for 30secs
  - calculate bandwith

Tickets
------------

- Bandwith Authority Renovation:
  https://trac.torproject.org/projects/tor/ticket/13630
- getting bandwidth servers more consistent and more geographically distributed:
  https://trac.torproject.org/projects/tor/ticket/24674
- TorFlow component query (63) : https://trac.torproject.org/projects/tor/query?status=!closed&component=Core+Tor%2FTorflow
- tor-bwauth keyword query (7): https://trac.torproject.org/projects/tor/query?status=!closed&keywords=~tor-bwauth

Class diagram
--------------

.. uml::

    @startuml
    namespace TorCtl {
        EventHandler <|-- ConsensusTracker
        EventHandler "0" *-- "*" EventListener
        ConsensusTracker <|-- PathBuilder
        PathBuilder *-- SelectionManager
        SelectionManager *-- PathSelector
        PathSelector "0" *-- "*" PathRestriction
        PathSelector "2" *-- "*" NodeGenerator
        NodeGenerator "0" *-- "*" NodeRestriction
    }
    namespace NetworkScanners {
      package SpeedRacer {}
      package BuildTimes {}
      package SoaT {}
    }
    namespace Statistics {
      package StatsHandler {}
    }
    @enduml


Docs
----

To run Bandwith authority scanner [#]_


Install & run
~~~~~~~~~~~~~~

::

    ./add_torctl.sh
    vim ./NetworkScanners/BwAuthority/data/scanner.1/bwauthority.cfg
    cd NetworkScanners/BwAuthority/
    ./setup.sh
    ./run_scan.sh

Example output
~~~~~~~~~~~~~~~~

::

    1515946341
    node_id=$0E5D730466E5BB675BC9704C374DBE7E19F06165 bw=57100 nick=artlogic measured_at=1515314555 updated_at=1515314555 pid_error=4.11374090719 pid_error_sum=4.11374090719 pid_bw=57136645 pid_delta=2.12168374577 circ_fail=0.2 scanner=/scanner.1/scan-data/bws-6.5:7.4-done-2018-01-07-02:42:35
    node_id=$DCC6C70E8F4340CBD633F506BC9E7D99818601D9 bw=47400 nick=eludemailrelay measured_at=1515666291 updated_at=1515666291 pid_error=3.96703337994 pid_error_sum=3.96703337994 pid_bw=47422125 pid_delta=2.65469736988 circ_fail=0.0 scanner=/scanner.1/scan-data/bws-0.8:1.6-done-2018-01-11-04:24:51

File format
------------

    2.4. Result format

    The final output file for use by the directory authorities is comprised of
    lines of the following format:

      "node_id=" fingerprint SP
      "bw=" new_bandwidth SP
      "nick=" nickname SP
      "measured_at=" slice timestamp NL

    If PID control is enabled, additional values are stored. See Section 3.4
    for those.

[#]_

    3.4. Value storage

       In order to maintain the PID information, we store the following additional
       fields in the output file:

          "pid_error=" (PID error term as defined in Section 3.1) SP
          "pid_error_sum=" (Weighted sum of PID error) SP
          "pid_delta=" (Change in error) SP
          "pid_bw=" (Last bandwidth value used in feedback) NL

       pid_delta is purely informational, and is not used in feedback.

[#]_

Bandwidth measurements files
-----------------------------

https://bwauth.ritter.vg/bwauth/

Measurements analysis
-----------------------

- map of bandwidth bias: https://atlas.torproject.org/#map_consensus_weight_to_bandwidth
- bandwidth authority variance: https://tomrittervg.github.io/bwauth-tools/
- CDF graphs of bw authority votes for all of the flag combinations:
  https://trac.torproject.org/projects/tor/ticket/2394,
  https://gitweb.torproject.org/metrics-tasks.git/tree/task-2394


.. [#] https://lists.torproject.org/pipermail/tor-dev/2017-December/012714.html
.. [#] https://gitweb.torproject.org/torflow.git/tree/NetworkScanners/BwAuthority/README.spec.txt#n332
.. [#] https://gitweb.torproject.org/torflow.git/tree/NetworkScanners/BwAuthority/README.spec.txt#n447
.. [#] https://trac.torproject.org/projects/tor/wiki/doc/BandwidthAuthority
