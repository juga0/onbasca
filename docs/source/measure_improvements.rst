.. _measure_improvements:

How to measure improvements
=============================

How to check bandwidth is not worst
--------------------------------------

* Using data from [OnionperfCode]_ and [TorperfCode]_, the time to download
  files should not be bigger. See [MetricsPerf]_
* Total relay bandwidth should not be smaller. See [MetricsBw]_
* Running tor with [shadow]_ and Cellstatistics enabled, the number of cells
  should be small. See :ref:`sbws_shadow`

Existing graphs
----------------

Metrics performance
~~~~~~~~~~~~~~~~~~~~~

.. image:: images/metrics_performance.png

[MetricsPerf]_

Metrics bandwidth (advertised vs history)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: images/bandwidth-2018-05-22-2018-08-20.png
   :width: 800px

[MetricsBw]_

.. image:: images/map_bw_uncharted.png
   :width: 800px

New bandwidth graphs ideas
---------------------------

* raw measured bw vs consensus bw
* desc avg-bw vs consensus bw
* desc obs-bw vs consensus bw

Other ideas
------------

* Firefox extension that tells current circuit bandwidth?
* Firefox extension that tells current measured capacity in the network?
