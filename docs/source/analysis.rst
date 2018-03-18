Analysis
=========

Existing software
-----------------

Code/papers
~~~~~~~~~~~~

1. Torflow: `Torflow code <https://gitweb.torproject.org/torflow.git>`_,
   `Torflow paper <https://research.torproject.org/techreports/torflow-2009-08-07.pdf>`_,
   specs: in the code repo,
   `wiki <https://trac.torproject.org/projects/tor/wiki/doc/BandwidthAuthority>`_
2. bwscanner: `bwscanner code <https://github.com/TheTorProject/bwscanner>`_
3. peerflow: `peerflow paper <https://ohmygodel.com/publications/peerflow-popets2017.pdf>`_
4. bridge bw scanner: `Intership annonuncement <https://blog.torproject.org/summer-2017-internship-create-bridge-bandwidth-scanner>`_,
   code ?
5. OnionPerf `OnionPerf code <https://github.com/robgjansen/onionperf>`_

Possible approaches
~~~~~~~~~~~~~~~~~~~~

Based on the discussions in [#]_ and [#]_:
a. Develop peerflow and deploy it in place of torflow
b. Finalize bwscanner and deploy in place of torflow
c. Adapt the bridge bw scanner that is currently being developed
d. Adapt OnionPerf
e. Path Torflow
f. Something simple from scratch

Evaluation:
a. Peerflow can be developed in parallel, it does not implement the simple algorithm needed in a short term
c. The bridge scanner is considered not suitable for converting into a bandwith scanner
d. OnionPerf uses Stem, it would be better to use Stem directly because OnionPerf implements other stuff not needed for the bandwith scanner
e. Patching Torflow is not considered acceptable because is hard to run and maintain
b. BwScanner can be fixed and finished but seems hard to mantain
f. Something simple from scratch can be implemented reusing BwScanner and txtorcon new features

.. [#] https://lists.torproject.org/pipermail/tor-dev/2017-December/012703.html
.. [#] https://trac.torproject.org/projects/tor/wiki/org/meetings/2018Rome/Notes/BandwidthAuthorityRequirements
