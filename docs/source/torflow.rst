.. _torflow:

Torflow in detail
=================

- by Mike Perry and others
- 2010?
- use TorCtl [pytorctlCode]_, Python 2
- not easy to install
- not easy to maintain

[TorflowCode]_, [TorflowPaper]_, [TORFLOWSPEC]_, [BwAuthWiki]_

How ``torflow`` performs measurements
--------------------------------------

- split relays into buckets by bandwidth percentile
- build two hop circuits with a relay and exit from relays in the bucket
- download a file from a Web server. The file size is based on the bucket
- measure how long it takes
- store the results

What ``torflow`` is doing, in a broad sense:
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

[mlBwProgressDec2]_

Algorithm to build two hops path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- take all relays, order by bandwith
- divide in consecutive slices of 50 relays
- for each relay in slice:
  - take a (random?) exit in the slice (from the set of exits in that slice minus the relay itself)
  - build a path with the relay and the exit
  - download a file for 30secs
  - calculate bandwith


How ``torflow`` aggregates measurements
----------------------------------------

- hourly
- calculate a bandwidth weight based on the descriptor ``observed bandwidth`` (advertised bandwidth ratio?)
- generate a ``measurements`` file

The ``measurements`` file is read by the dirauths and includes the ``measured``
bandwidth in their votes.

How the bandwidth weight is calculated
---------------------------------------

::

    bw_observed = min(bandwidth-avg, bandwidth-burst, bandwidth-observed)

Example ``torflow``'s Bandwidth measurements file
--------------------------------------------------

`aka` `Bandwidth list v1.0.0` 

.. code-block:: none

  1523911758
  node_id=$68A483E05A2ABDCA6DA5A3EF8DB5177638A27F80 bw=760 nick=Test measured_at=1523911725 updated_at=1523911725 pid_error=4.11374090719 pid_error_sum=4.11374090719 pid_bw=57136645 pid_delta=2.12168374577 circ_fail=0.2 scanner=/filepath
  node_id=$96C15995F30895689291F455587BD94CA427B6FC bw=189 nick=Test2 measured_at=1523911623 updated_at=1523911623 pid_error=3.96703337994 pid_error_sum=3.96703337994 pid_bw=47422125 pid_delta=2.65469736988 circ_fail=0.0 scanner=/filepath

Tickets
------------

- Bandwith Authority Renovation: [Ticket13630]_
- getting bandwidth servers more consistent and more geographically distributed: [Ticket24674]_
- ``torflow`` component query (63) : [TorflowComp]_
- tor-bwauth keyword query (7): [BwAuthKey]_

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

To run Bandwith authority scanner [BwAuthWiki]_


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

.. code-block:: none

    2.4. Result format

    The final output file for use by the directory authorities is comprised of
    lines of the following format:

      "node_id=" fingerprint SP
      "bw=" new_bandwidth SP
      "nick=" nickname SP
      "measured_at=" slice timestamp NL

    If PID control is enabled, additional values are stored. See Section 3.4
    for those.

[TORFLOWSPEC332]_

.. code-block:: none

    3.4. Value storage

       In order to maintain the PID information, we store the following additional
       fields in the output file:

          "pid_error=" (PID error term as defined in Section 3.1) SP
          "pid_error_sum=" (Weighted sum of PID error) SP
          "pid_delta=" (Change in error) SP
          "pid_bw=" (Last bandwidth value used in feedback) NL

       pid_delta is purely informational, and is not used in feedback.

[TORFLOWSPEC447]_

Scaling
--------

See :ref:`torflow_aggr`

Bandwidth measurements files
-----------------------------

[BwAuthFiles]_

Measurements analysis
-----------------------

- map of bandwidth bias: [MetricsWeight]_
- bandwidth authority variance: [BwAuthTools]_
- CDF graphs of bw authority votes for all of the flag combinations:
  [Ticket2394]_,
  [MetricsTask2394]_
