.. _index:

OnBaSca ⚖🌏
============

Tor bandwidth authorities run bandwidth scanners to measure the relays capacity
in the Tor network. They send their results to the directory authorities,
which use them to calculate the bandwidth weights of the relays,
what influences how the paths between relays and built.

The bandwidth scanners play and important role on the performance and
security of the Tor network.

This project aims at improving the current bandwidth scanner implementation
and research on the current issues in the network related to
bandwidth measurements and weights.

The bandwidth scanner being improved is ``sbws``:

- `sbws <https://sbws.readthedocs.io/>`_ documentation
- `sbws code <https://gitweb.torproject.org/sbws.git>`_
- `sbws issues <https://trac.torproject.org/projects/tor/query?component=Core+Tor%2Fsbws&col=id&col=summary&col=component&col=status&col=type&col=priority&col=milestone&col=changetime&report=68&order=priority>`_

The improvements on core-Tor:

- `tor code <https://gitweb.torproject.org/tor.git>`_
- `bandwidth related tor tickets <https://trac.torproject.org/projects/tor/query?keywords=~bwauth&or&keywords=~dir-bwauth&col=id&col=summary&col=status&col=type&col=priority&col=milestone&col=component&order=priority>`_
- `bandwidth related wike page <https://trac.torproject.org/projects/tor/wiki/org/teams/NetworkTeam/BandwidthAuthority>`_

**Slides**:

- `Bornhack 2018 <https://juga0.github.io/onbasca_hax_slides/>`_

**This site** compiles information related to bandwidth in Tor, the current status
and the improvements, but the main documentation is on the Websites
of the software being improved.

For any issues or suggestions, please open an `issue <https://github.com/juga0/onbasca/issues>`_

You can clone this documentation on `Github <https://github.com/juga0/onbasca>`_,
or download it as `pdf <https://media.readthedocs.org/pdf/onbasca/latest/onbasca.pdf>`_
or `epub <https://readthedocs.org/projects/onbasca/downloads/epub/latest/>`_

.. toctree::
   :maxdepth: 3
   :caption: Documentation in **this site** about Tor bandwidth and scanners:
   :numbered:

   introduction
   bandwidth_tor
   bandwidth_tor_code
   bwscanner
   torflow
   torflow_aggr
   sbws
   distribution_goal
   measure_improvements
   thread_modeling
   analysis
   requirements
   open_questions
   planned_work
   agenda_tor_mx
   glossary
   references

.. toctree::
   :maxdepth: 3
   :caption: Useful documentation copied from other resources:

   tor_tests
   networks
   statistics
   pidcontrol
   dir-spec

* :ref:`genindex`
