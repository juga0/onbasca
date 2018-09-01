.. _glossary:

Glossary
=========

:ref:`<sbwsdoc:glossary>`

.. note::
    `Tor metrics <.https://metrics.torproject.org/glossary.html>`_ in rst.

.. glossary::

  advertised bandwidth:
    the volume of traffic, both incoming and outgoing, that a relay is willing to sustain, as configured by the operator and claimed to be observed from recent data transfers.

  bandwidth history:
    the volume of incoming and/or outgoing traffic that a relay claims to have handled on behalf of clients.

  bridge:
    a relay whose existence is non-public and which can therefore provide access for blocked clients, often in combination with pluggable transports, which registers itself with the bridge authority.

  bridge authority:
    a special-purpose relay that maintains a list of bridges as input for external bridge distribution mechanisms (for example, BridgeDB).

  circuit:
    a path through the Tor network built by clients starting with a bridge or relay and optionally continued by additional relays to hide the source of the circuit.

  client:
    a node in the Tor network, typically running on behalf of one user, that routes application connections over a series of relays.

  consensus:
    a single document compiled and voted on by the directory authorities once per hour, ensuring that all clients have the same information about the relays that make up the Tor network.

  consensus weight:
    a value assigned to a relay that is based on bandwidth observed by the relay and bandwidth measured by the directory authorities, included in the hourly published consensus, and used by clients to select relays for their circuits.

  directory authority:
    a special-purpose relay that maintains a list of currently-running relays and periodically publishes a consensus together with the other directory authorities.

  directory mirror:
    a relay that provides a recent copy of directory information to clients, in order to reduce the load on directory authorities.

  onion service:
    a service (for example, a website or instant-messaging server) that is only accessible via the Tor network.

  pluggable transport:
    an alternative transport protocol provided by bridges and used by clients to circumvent transport-level blockings (for example, by ISPs or governments).

  relay:
    a publicly-listed node in the Tor network that forwards traffic on behalf of clients, and that registers itself with the directory authorities.

  relay flag:
    a special (dis-)qualification of relays for circuit positions (for example, "Guard", "Exit", "BadExit"), circuit properties (for example, "Fast", "Stable"), or roles (for example, "Authority", "HSDir"), as assigned by the directory authorities and further defined in the directory protocol specification.

.. note::
    https://gitweb.torproject.org/torspec.git/tree/glossary.txt

.. glossary::

  ORPort 
    Onion Router Port
    
  DirPort 
    Directory Port

  Relays, 
    aka OR (onion router)

  Exit relay:
    The final hop in an exit circuit before traffic leaves
    the Tor network to connect to external servers.

  Non-exit relay:
    Relays that send and receive traffic only to
    other Tor relays and Tor clients.

  Entry relay:
    The first hop in a Tor circuit. Can be either a guard
    relay or a bridge, depending on the client's configuration.

  Guard relay:
    A relay that a client uses as its entry for a longer
    period of time.  Guard relays are rotated more slowly to prevent
    attacks that can come from being exposed to too many guards.

  Bridge:
    A relay intentionally not listed in the public Tor
    consensus, with the purpose of circumventing entities (such as
    governments or ISPs) seeking to block clients from using Tor.
    Currently, bridges are used only as entry relays.

  Directory cache:
    A relay that downloads cached directory information
    from the directory authorities and serves it to clients on demand.
    Any relay will act as a directory cache, if its bandwidth is high enough.

  Rendezvous point:
    A relay connecting a client to a hidden service.
    Each party builds a three-hop circuit, meeting at the
    rendezvous point.

  Directory Authority:
    Nine total in the Tor network, operated by
    trusted individuals. Directory authorities define and serve the
    consensus document, defining the "state of the network." This document
    contains a "router status" section for every relay currently
    in the network. Directory authorities also serve router descriptors,
    extra info documents, microdescriptors, and the microdescriptor consensus.

  Bridge Authority:
    One total. Similar in responsibility to directory
    authorities, but for bridges.

  Fallback directory mirror:
    One of a list of directory caches distributed
    with the Tor software. (When a client first connects to the network, and
    has no directory information, it asks a fallback directory. From then on,
    the client can ask any directory cache that's listed in the directory
    information it has.)

  Hidden Service:
   A hidden service is a server that will only accept incoming
   connections via the hidden service protocol. Connection
   initiators will not be able to learn the IP address of the hidden
   service, allowing the hidden service to receive incoming connections,
   serve content, etc, while preserving its location anonymity.

  Circuit:
   An established path through the network, where cryptographic keys
   are negotiated using the ntor protocol or TAP (Tor Authentication
   Protocol (deprecated)) with each hop. Circuits can differ in length
   depending on their purpose. See also Leaky Pipe Topology.

  Exit Circuit:
    A circuit which connects clients to destinations
    outside the Tor network. For example, if a client wanted to visit
    duckduckgo.com, this connection would require an exit circuit.

  Internal Circuit:
    A circuit whose traffic never leaves the Tor
    network. For example, a client could connect to a hidden service via
    an internal circuit.
    
  Consensus:
     The state of the Tor network, published every hour,
     decided by a vote from the network's directory authorities. Clients
     fetch the consensus from directory authorities, fallback
     directories, or directory caches.

   Descriptor:
    Each descriptor represents information about one
    relay in the Tor network. The descriptor includes the relay's IP
    address, public keys, and other data. Relays send
    descriptors to directory authorities, who vote and publish a
    summary of them in the network consensus.

  Link handshake
      The link handshake establishes the TLS connection over which two
      Tor participants will send Tor cells.  This handshake also
      authenticates the participants to each other, possibly using Tor
      cells.

  Circuit handshake
    Circuit handshakes establish the hop-by-hop onion encryption
    that clients use to tunnel their application traffic.  The
    client does a pairwise key establishment handshake with each
    individual relay in the circuit.  For every hop except the
    first, these handshakes tunnel through existing hops in the
    circuit.  Each cell type in this protocol also has a newer
    version (with a "2" suffix), e.g., CREATE2.

  CREATE cell:
    First part of a handshake, sent by the initiator.

  CREATED cell:
    Second part of a handshake, sent by the responder.

  EXTEND cell:
    (also known as a RELAY_EXTEND cell) First part of a
    handshake, tunneled through an existing circuit.  The last relay
    in the circuit so far will decrypt this cell and send the
    payload in a CREATED cell to the chosen next hop relay.

  EXTENDED cell:
    (also known as a RELAY_EXTENDED cell) Second part
    of a handshake, tunneled through an existing circuit.  The last
    relay in the circuit so far receives the CREATED cell from the
    new last hop relay and encrypts the payload in an EXTENDED cell
    to tunnel back to the client.

  Onion skin:
    A CREATE/CREATE2 or EXTEND/EXTEND2 payload that
    contains the first part of the TAP or ntor key establishment
    handshake.

  Leaky Pipe Topology:
    The ability for the origin of a circuit to address
    relay cells to be addressed to any hop in the path of a circuit. In Tor,
    the destination hop is determined by using the 'recognized' field of relay
    cells.

  Stream:
    A single application-level connection or request, multiplexed over
    a Tor circuit.  A 'Stream' can currently carry the contents of a TCP
    connection, a DNS request, or a Tor directory request.

  Channel:
    A pairwise connection between two Tor relays, or between a
    client and a relay. Circuits are multiplexed over Channels. All
    channels are currently implemented as TLS connections.


See also `Tor glossary version 1.0 <https://trac.torproject.org/projects/tor/wiki/doc/community/glossary>`_