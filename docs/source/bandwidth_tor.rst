.. _bandwidth_tor:

Bandwidth values in Tor (Aug 2018)
===================================

.. _bandwidth_tor_conf:

Bandwidth values in relays' configuration
------------------------------------------

``/etc/tor/torrc``

* BandwidthRate
  A token bucket limits the average incoming/outgoing bandwidth (default 1GB)
* BandwidthBurst
  Limit the maximum token bucket size (also known as the burst) in each direction (default: 1GB)
* MaxAdvertisedBandwidth
  will not advertise more than this amount of bandwidth for our BandwidthRate
  Server operators who want to reduce the number of clients who ask
  to build circuits through them (since this is proportional to advertised bandwidth rate) can thus reduce the CPU demands on their server without impacting
  network performance.
* RelayBandwidthRate
* RelayBandwidthBurst
* PerConnBWRate
* PerConnBWBurst

[TOR1]_

.. _bandwidth_tor_desc:

Bandwidth values in relays' descriptor
---------------------------------------

* ``average bandwidth`` (not any measurements)
  bw that the relay is willing to sustain over long periods
* ``burst bandwidth``
  bw that the relay is willing to sustain in very short intervals
* ``observed bandwidth``
  min(max bandwidth sustained output over any 10secs in the past day,
  another sustained input)

.. code-block:: none

  "bandwidth" bandwidth-avg bandwidth-burst bandwidth-observed NL

[DIRSPEC427]_

From tor code:

.. code-block:: none

   bandwidth-avg = bandwidtrate = get_effective_bwrate()
                 = min(RelayBandwidthRate, BandwidthRate, MaxAdvertisedBandwidth)
   bandwidth-burst = bandwidthburst = get_effective_bwburst()
                   = min(RelayBandwidthBust, BandwidthBurst)
   bandwidth-observed = bandwidthcapacity = rep_hist_bandwidth_assess()

.. code-block:: none

  advertised bandwidth = router_get_advertised_bandwidth_capped()
                       = min(bandwidtrate, bandwidthcapacity, 10MB/s)

``advertised bandwidth`` is not mentioned in the spec, but it is used in the
code to calculate the consensus bandwidth. See below.

.. _bandwidth_tor_cons:

Bandwidth values in dirauths' consensus documents
--------------------------------------------------

.. code-block:: none

  "w" SP "Bandwidth=" INT [SP "Measured=" INT] [SP "Unmeasured=1"] NL

  The bandwidth in a “w” line should be [...]
  the lesser of the observed bandwidth and bandwidth rate limit from the
  server descriptor.
  It is given in kilobytes per second, and capped at some arbitrary value
  (currently 10 MB/s).

  If 3 or more authorities provide a Measured= keyword for
  a router, the authorities produce a consensus containing a "w"
  Bandwidth= keyword equal to the median of the Measured= votes.

  The Measured= keyword on a “w” line vote is currently computed by
  multiplying the previous published consensus bandwidth by the ratio
  of the measured average node stream capacity to the network average

[DIRSPEC2337]_

.. code-block:: none

  bandwidth rate limit = bandwidth-avg
  consensus bandwidth = Bandwidth

``bandwidth rate limit`` appears here for 1st time to refer to the
``bandwidth average``.

.. code-block:: none

  Bandwidth = advertised bandwidth
            = min(bandwidth-avg, bandwidth-observed, 10MB/s) (KB/s)

If 3 or more authorities provide a Measured= keyword::

  Bandwidth = consensus bandwidth * ratio(avg stream, network avg)

``consensus bandwidth`` appears here for 1st time to refer to the consensus
``Bandwidth``.
``ratio of the measured average node stream capacity to the network average``
refers to the calculation that Torflow does, which currently (no PID) is

.. math::

    bwn_i =&
        max\left(
            \frac{bw_i}{\mu},
            \frac{bwf_i}{\mu_{bwf}}
            \right)
        \times bwobs_i

That is, the maximum between the ratio of the measured stream bandwidth by
the network average stream bandwidth and the ratio of the filtered bandwidth
by the network average filtered bandwidth multiplied by the descriptor
``observed bandwidth`` (not previous ``consenus bandwidth``)::

  Bandwidth = observed bandwidth * (stream bandwidth / stream mu)

See :ref:`torflow_aggr` form more details



Bandwidth values origin
------------------------------

.. image:: images/bw_values.png
