.. _introduction:

Introduction to Tor bandwidth scanner
======================================

.. toctree: Contents
   :local:
   :titlesonly:

Brief introduction to Tor
---------------------------

.. image:: images/1058px-How_Tor_Works_2.svg.png

Directory Authorities
----------------------

.. code-block:: none

  a special-purpose relay that maintains a list of currently-running relays
  and periodically publishes a consensus together with the other 
  directory authorities.

[#]_

.. image:: images/dir_auth.png

[#]_

Consensus documents
-------------------

.. code-block:: none

    a single document compiled and voted on by the directory authorities once 
    per hour, ensuring that all clients have the same information about the relays
    that make up the Tor network.

[#]_

.. image:: images/consensus.png

[#]_

Why is needed to have a consensus documents?
:::::::::::::::::::::::::::::::::::::::::::::

so that all relays have same view of the network

How is consensus created
:::::::::::::::::::::::::

#. relays publish descriptors to a directory authority (dirauth)
#. dirauths exchange descriptors with each other
#. the dirauths that have measurements from a bandwidth scanner (4/10):

   #. read those measurements
   #. calculate bandwidth weights
   #. create vote documents including the `measured` bandwidth and consensus weight
#. dirauths exchange votes 
#. dirauths create consensus documents
#. dirauths publish consensus documents

Why is needed to have relays' bandwidth measurements (aka scanners)?
---------------------------------------------------------------------

* to distribute the network load according to relay capacity
* relays self-advertise their bandwidths, but a malicious relay could 
  self-advertise more bandwidth in order to get more circuits to it.
  Having more circuits makes easier some attacks (source routed?)

What a better bandwidth scanner could improve
-----------------------------------------------

* distribute the network load smarter making Tor network faster and more fair
* increase the diversity of the relays (currently most of the bandwidth is in Germany)
* encourage relay operators to increase the capacity of their relays 

See :ref:analysis and :ref:requirements for more details.

.. rubric:: Footnotes

.. [#] https://metrics.torproject.org/glossary.html
.. [#] https://metrics.torproject.org/glossary.html
.. [#] https://metrics.torproject.org/rs.html#search/flag:Authority
.. [#] https://jordan-wright.com/blog/images/blog/how_tor_works/consensus.png
