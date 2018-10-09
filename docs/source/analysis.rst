.. _analysis:

Analysis (Mar 2018)
====================

Existing software
-----------------

1. ``torflow``: :ref:`torflow`
2. ``bwscanner``: :ref:`bwscanner`
3. ``peerflow``: [PeerflowPaper]_
4. bridge bw scanner: [BridgeBwScanner]_
5. ``OnionPerf`` [OnionperfCode]_

Possible approaches
--------------------

Based on the discussions in [mlBwProgressDec1]_ and [BwAuthRome]_:

a. Develop ``peerflow`` and deploy it in place of ``torflow``
b. Finalize ``bwscanner`` and deploy in place of ``torflow``
c. Adapt the bridge bw scanner that is currently being developed
d. Adapt ``OnionPerf``
e. Path ``torflow``
f. Something simple from scratch

Evaluation:

a. ``Peerflow`` can be developed in parallel, it does not implement the simple algorithm needed in a short term
b. ``bwscanner`` can be fixed and finished but seems hard to mantain
c. The bridge scanner is considered not suitable for converting into a bandwith scanner
d. ``OnionPerf`` uses Stem, it would be better to use Stem directly because OnionPerf implements other stuff not needed for the bandwith scanner
e. Patching ``torflow`` is not considered acceptable because is hard to run and maintain
f. Something simple from scratch can be implemented reusing ``bwscanner`` and ``txtorcon`` new features
