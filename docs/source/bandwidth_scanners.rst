.. _bandwidth_scanners:

Current status of bandwidth scanners (Aug 2018)
===============================================

.. _torflow-status:

``torflow``
-----------

- by Mike Perry and others
- 2010?
- use TorCtl [#]_, Python 2
- not easy to install
- not easy to maintain

https://gitweb.torproject.org/torflow.git

https://research.torproject.org/techreports/torflow-2009-08-07.pdf

How ``torflow`` performs measurements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- split relays into buckets by bandwidth percentile
- build two hop circuits with a relay and exit from relays in the bucket
- download a file from a Web server. The file size is based on the bucket
- measure how long it takes
- store the results

How ``torflow`` aggregates measurements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- hourly
- calculate a bandwidth weight based on the descriptor ``observed bandwidth`` (advertised bandwidth ratio?)
- generate a ``measurements`` file

The ``measurements`` file is read by the dirauths and includes the ``measured``
bandwidth in their votes.

How the bandwidth weight is calculated
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    bw_observed = min(bandwidth-avg, bandwidth-burst, bandwidth-observed)
    
.. math::

    bwnew_i &=
        max\left(
            \frac{bw_i}{\mu},
            min \left(
                bw_i,
                bw_i \times \mu
                \right)
                    \times
                    \frac{bw}{\sum_{i=1}^{n}
                    min \left(bw_i,
                        bw_i \times \mu
                \right)}
            \right)
        \times bwdescobs_i \\
    &=
        max\left(
            \frac{bw_i}{\frac{\sum_{i=1}^{n}bw_i}{n}},
            min \left(
                bw_i,
                bw_i \times \frac{\sum_{i=1}^{n}bw_i}{n}
                \right)
                    \times
                    \frac{bw}{\sum_{i=1}^{n}
                    min \left(bw_i,
                        bw_i \times \frac{\sum_{i=1}^{n}bw_i}{n}
                \right)}
            \right)
        \times bwdescobs_i

Example ``torflow``'s Bandwidth measurements file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`aka` `Bandwidth list v1.0.0` 

.. code-block:: none

  1523911758
  node_id=$68A483E05A2ABDCA6DA5A3EF8DB5177638A27F80 bw=760 nick=Test measured_at=1523911725 updated_at=1523911725 pid_error=4.11374090719 pid_error_sum=4.11374090719 pid_bw=57136645 pid_delta=2.12168374577 circ_fail=0.2 scanner=/filepath
  node_id=$96C15995F30895689291F455587BD94CA427B6FC bw=189 nick=Test2 measured_at=1523911623 updated_at=1523911623 pid_error=3.96703337994 pid_error_sum=3.96703337994 pid_bw=47422125 pid_delta=2.65469736988 circ_fail=0.0 scanner=/filepath

See :ref:`torflow_detail` for more details about ``torflow``

.. _bwscanner-status:

``bwscanner``
-------------

- by Aaron Gibson and others
- 2016
- use `twisted <https://twistedmatrix.com/>`_ and `txtorcon <https://github.com/meejah/txtorcon>`_
- not production ready
- currently unmaintained

https://github.com/TheTorProject/bwscanner

See also :ref:`bwscanner`

.. _sbws-status:

Simple bandwidth scanner (``sbws``)
------------------------------------

- by Matt Traudt and juga
- 2018
- use threads
- use `stem <https://gitweb.torproject.org/stem.git>`_, Python 3

https://gitweb.torproject.org/sbws.git

Initial ``sbws`` design
~~~~~~~~~~~~~~~~~~~~~~~~

exit relays were `helper relays`: 
relays with high bandwidth that would allow to exit only to
the Web server

- who should own these exits?
- extra infrastructure 

Which relays are measured first
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ``sbws`` prioritizes relays to be measured
  - new relays
  - relays not measured recently

How ``sbws`` perform measurements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- build two hop circuits with the relay to measure and a random exit
  that has similar advertised bandwidth as the relay to measure
- download a file from a Web server
- the file size is adjusted depending on whether it takes more or less time 
  to download the file
- measure how long it takes
- store the results

.. image:: images/scanner.png
  
``sbws`` raw measurements compared to ``torflow`` measurements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: images/43710932-ac1eeea8-9960-11e8-9e7e-21fddff2f7a3.png

.. image:: images/43710933-ac95e0bc-9960-11e8-9aaf-0bb1f83b65e2.png

How ``sbws`` aggregate the results
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- scaling
- imitate ``torflow`` scaling

``sbws`` linear scaling
:::::::::::::::::::::::::

Multiply each relay bandwidth by `7500/median`

.. image:: images/20180901_163442.png

``sbws`` scaling as ``torflow``
:::::::::::::::::::::::::::::::

Multiply each relay observed bandwidth by a ratio calculated from the 
measured bandwdiths

.. image:: images/20180901_164014.png

Example sbws's bandwidth list file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

aka v1.1.0 bandwidth list file

.. code-block:: none

  1523911758
  version=1.1.0
  software=sbws
  software_version=0.1.0
  latest_bandwidth=2018-04-16T20:49:18
  file_created=2018-04-16T21:49:18
  generator_started=2018-04-16T15:13:25
  earliest_bandwidth=2018-04-16T15:13:26
  ====
  bw=380 error_circ=0 error_misc=0 error_stream=1 master_key_ed25519=YaqV4vbvPYKucElk297eVdNArDz9HtIwUoIeo0+cVIpQ nick=Test node_id=$68A483E05A2ABDCA6DA5A3EF8DB5177638A27F80 rtt=380 success=1 time=2018-05-08T16:13:26
  bw=189 error_circ=0 error_misc=0 error_stream=0 master_key_ed25519=a6a+dZadrQBtfSbmQkP7j2ardCmLnm5NJ4ZzkvDxbo0I nick=Test2 node_id=$96C15995F30895689291F455587BD94CA427B6FC rtt=378 success=1 time=2018-05-08T16:13:36

https://gitweb.torproject.org/torspec.git/tree/bandwidth-file-spec.txt


Dirauths' operating system distributions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: images/Debian-OpenLogo.svg
.. image:: images/OpenBSD.svg
.. image:: images/Freebsd_logo.svg

``sbws`` system packages
~~~~~~~~~~~~~~~~~~~~~~~~

- Debian package (https://salsa.debian.org/pkg-privacy-team/sbws/)
  - waiting Debian ftp master to approve it
  - waiting for new `stem` release and package update
- OpenBSD Makefile (https://github.com/juga0/sbws_openbsd)

``sbws`` diagrams
~~~~~~~~~~~~~~~~~~

:ref:`Diagrams<sbwsdoc:diagrams>`

https://sbws.readthedocs.io/en/latest/diagrams.html


.. rubric:: Footnotes

.. [#] https://gitweb.torproject.org/pytorctl.git/