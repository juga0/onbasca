.. _bandwidth_tor_code:

Bandwidth code in ``tor``
==========================

Descriptors
------------

``bandwithrate`` is used to name ``bandwidth-avg`` in dir-spec.txt 2.1.1

.. code-block:: none

  bandwidth-avg = min(RelayBandwidthRate, RelayBandwidthBust, BandwidthRate, BandwidthBurst, MaxAdvertisedBandwidth)

  bandwidth-burst = min(RelayBandwidthBust, BandwidthBurst)

From ``get_effective_bwrate()`` [#]_ and ``get_effective_bwburst()``

``bandwidthcapacity == ``bandwidth-observed`` (dir-spec)

From ``rep_hist_bandwidth_asses`` that reads from ``bw_array_t``
Example::

    bandwidthcapacity = max?(r 10547200, w 10536960)



bandwidthcapacity 
------------------

``/or/router.c``:

.. code-block:: c

    router_perform_bandwidth_test

    router_build_fresh_descriptor

    /* compute ri->bandwidthrate as the min of various options */
    ri->bandwidthrate = get_effective_bwrate(options);
    ri->bandwidthburst = get_effective_bwburst(options);
    ri->bandwidthcapacity = hibernating ? 0 : rep_hist_bandwidth_assess();

    router_dump_router_to_string
      (int) router->bandwidthrate,
      (int) router->bandwidthburst,
      (int) router->bandwidthcapacity,

``/or/rephist.c``:

.. code-block:: c

    rep_hist_bandwidth_asses

    commit_max


``/or/dirserv.c``:

.. code-block:: c

    set_routerstatus_from_routerinfo

    dirserv_get_credible_bandwidth_kb
    
measured bandwidth (by bwauths) or the self advertised bandwidth, but not any self measured bandwidth

``/or/dirvote.c``:

.. code-block:: c

    networkstatus_compute_consensus
      
    smartlist_add_asprintf(chunks, "w Bandwidth=%d%s%s\n",
                             rs_out.bandwidth_kb,
                             unmeasured?" Unmeasured=1":"",
                             guardfraction_str ? guardfraction_str : "");


Vote and consensus
-------------------

``Bandwidth`` in dir-spec.txt 3.4.1 votes::

    Bandwidth = min(bandwidth-avg, bandwidth-observed)?
    
Constants 
-----------

.. code-block:: c

    DEFAULT_MAX_UNMEASURED_BW_KB 20

    BANDWIDTH_CHANGE_FACTOR 2

    NUM_SECS_ROLLING_MEASURE 10 /* secs */


    NUM_SECS_BW_SUM_IS_VALID (5*24*60*60) /* 5 days */
    NUM_SECS_BW_SUM_INTERVAL (24*60*60) /* 24 hours */
    MAX_UPTIME_BANDWIDTH_CHANGE (24*60*60)
    MAX_BANDWIDTH_CHANGE_FREQ (3*60*60) /* 3 hours */

    NUM_TOTALS = NUM_SECS_BW_SUM_IS_VALID / NUM_SECS_BW_SUM_INTERVAL  = 5
    NUM_SECS_BW_SUM_INTERVAL * NUM_TOTALS = NUM_SECS_BW_SUM_IS_VALID (5 days)


.. rubric:: Footnotes

.. [#] http://juga.space/tor_doxygen/config_8c.html#ae937a27a04bbab82090c0d47c5846309