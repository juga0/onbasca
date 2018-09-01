.. _bandwidth_tor:

Bandwidth values in Tor (Aug 2018)
===================================

- relays' self-measured bandwidth 
- relays' operator configured bandwidth
- ``scanner``/``generator`` relays' measured bandwidth
- dirauths consensus weights

Bandwidth values in relays' descriptor
---------------------------------------

* ``average bandwidth`` (not any measurements)
  bw that the relay is willing to sustain over long periods
* ``burst bandwidth``
  bw that the relay is willing to sustain in very short intervals
* ``observed bandwidth``
  min(max bandwidth sustained output over any 10secs in the past day,
  another sustained input)


.. note:: which of these is the ``advertised bandwidth``?

.. code-block:: none

  "bandwidth" bandwidth-avg bandwidth-burst bandwidth-observed NL

[#]_

From tor code:

.. code-block:: none

   bandwidth-avg = min(RelayBandwidthRate, RelayBandwidthBust, BandwidthRate, BandwidthBurst, MaxAdvertisedBandwidth)
   bandwidth-burst = min(RelayBandwidthBust, BandwidthBurst)

Descriptor example
~~~~~~~~~~~~~~~~~~~

.. code-block:: none

  @downloaded-at 2018-03-26 07:05:10
  @source "86.59.21.38"
  router sonsorolDotNet 50.247.195.124 9001 0 9030
  identity-ed25519
  -----BEGIN ED25519 CERT-----
  AQQABnPkARHcXgsN0T0t2BXquO7Bc5E2ZgtxlGD3mGaJIyct3wITAQAgBAAx8gv4
  geoonl/0flvSzrNoBTE0O9/ZhqDHhaGXFqkH++Yg3b9CUG33sPYc7a66sc8UBK87
  Br9/7HR/67fyCyOqngcxNdBxFf1fKLImej/kLvWuHXpA1dcgZdB5uPZ9dQM=
  -----END ED25519 CERT-----
  master-key-ed25519 MfIL+IHqKJ5f9H5b0s6zaAUxNDvf2Yagx4WhlxapB/s
  platform Tor 0.3.3.3-alpha on Linux
  proto Cons=1-2 Desc=1-2 DirCache=1-2 HSDir=1-2 HSIntro=3-4 HSRend=1-2 Link=1-5 LinkAuth=1,3 Microdesc=1-2 Relay=1-2
  published 2018-03-26 06:48:42
  fingerprint 3DE5 67C1 350C 0E85 8C61 47AE CB06 EA9B 3EAF 3261
  uptime 1944246
  bandwidth 819200 1638400 1693124
  extra-info-digest D717F6C073391E11FFE81CF8A08E650788286842 CcKt5jLKC7bikmAtX59GN/xKmANKCXwGEi3KGW2FVEI
  onion-key
  -----BEGIN RSA PUBLIC KEY-----`

``/var/lib/tor/cached-descriptors``

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

[#]_

Bandwidth values in dirauths' consensus documents
--------------------------------------------------

.. code-block:: none

  "w" SP "Bandwidth=" INT [SP "Measured=" INT] [SP "Unmeasured=1"] NL

[#]_

Bandwidth = min(observed bandwidth, descriptor bandwidth rate limit, 10MB/s) 
KB/s

Measured = consensus bandwidth * ratio(avg stream, "network avg"?)

.. code-block:: none

  If 3 or more authorities provide a Measured= keyword for
  a router, the authorities produce a consensus containing a "w"
  Bandwidth= keyword equal to the median of the Measured= votes.

Consensus example
~~~~~~~~~~~~~~~~~~

.. code-block:: none

  r CalyxInstitute14 ABG9JIWtRdmE7EFZyI/AZuXjMA4 fXwv8Ev8WzKw04I6y7k+7EqMUuU 2018-03-25 16:13:41 162.247.72.201 443 80
  s Exit Fast HSDir Running Stable V2Dir Valid
  v Tor 0.3.2.10
  pr Cons=1-2 Desc=1-2 DirCache=1-2 HSDir=1-2 HSIntro=3-4 HSRend=1-2 Link=1-5 LinkAuth=1,3 Microdesc=1-2 Relay=1-2
  w Bandwidth=6630
  p accept 20-23,43,53,79-81,88,110,143,194,220,389,443,464,531,543-544,554,563,636,706,749,873,902-904,981,989-995,1194,1220,1293,1500,1533,1677,1723,1755,1863,2082-2083,2086-2087,2095-2096,2102-2104,3128,3389,3690,4321,4643,5050,5190,5222-5223,5228,5900,6660-6669,6679,6697,8000,8008,8074,8080,8087-8088,8332-8333,8443,8888,9418,9999-10000,11371,12350,19294,19638,23456,33033,64738

.. code-block:: none

  r UbuntuCore212 TVya35Vd7ACGa7841ERSlX8PJKw byKPgLBokqHYUiHvIcIxbUK4qTc 2018-03-26 02:48:01 24.197.119.19 41935 0
  s Running V2Dir Valid
  v Tor 0.3.1.10
  pr Cons=1-2 Desc=1-2 DirCache=1-2 HSDir=1-2 HSIntro=3-4 HSRend=1-2 Link=1-5 LinkAuth=1,3 Microdesc=1-2 Relay=1-2
  w Bandwidth=0 Unmeasured=1
  p reject 1-65535

.. rubric:: Footnotes

.. [#] https://gitweb.torproject.org/torspec.git/tree/dir-spec.txt#n427
.. [#] https://gitweb.torproject.org/tor.git/tree/doc/tor.1.txt#n222
.. [#] https://gitweb.torproject.org/torspec.git/tree/dir-spec.txt#n2337