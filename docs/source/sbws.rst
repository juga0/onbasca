.. _sbws:

Simple bandwidth scanner (``sbws``)
------------------------------------

- by Matt Traudt and juga
- 2018
- use threads
- use [StemCode]_, Python 3

[sbwsCode]_, [sbwsDoc]_

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

aka v1.1.0 bandwidth list file [BWSPEC]_

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


Dirauths' operating system distributions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: images/Debian-OpenLogo.svg
   :width: 100px
.. image:: images/OpenBSD.svg
   :width: 100px
.. image:: images/Freebsd_logo.svg
   :width: 100px

``sbws`` system packages
~~~~~~~~~~~~~~~~~~~~~~~~

- Debian package [sbwsDeb]_
  - waiting Debian ftp master to approve it
  - waiting for new `stem` release and package update
- OpenBSD Makefile [sbwsBSD]_

``sbws`` diagrams
~~~~~~~~~~~~~~~~~~

:ref:`Diagrams <sbwsdoc:diagrams>`  [sbwsDia]_
