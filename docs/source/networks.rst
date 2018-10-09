Network  background
-------------------------------------

The Token Bucket Algorithm
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The token bucket algorithm can be conceptually understood as follows::

    A token is added to the bucket every 1 / r seconds.
    The bucket can hold at the most b tokens. If a token arrives when the bucket is full, it is discarded.
    When a packet (network layer PDU) of n bytes arrives,
        if at least n tokens are in the bucket, n tokens are removed from the bucket, and the packet is sent to the network.
        if fewer than n tokens are available, no tokens are removed from the bucket, and the packet is considered to be non-conformant.

Burst size

Let M be the maximum possible transmission rate in bytes/second.

Then T max = { b / ( M − r )  if  r < M ∞  otherwise is the maximum burst time, that is the time for which the rate M is fully utilized.

The maximum burst size is thus B max = T max ∗ M 

[WPToken]_

network throughput
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

is the rate of successful message delivery over a communication channel. The data these messages belong to may be delivered over a physical or logical link, or it can pass through a certain network node. Throughput is usually measured in bits per second (bit/s or bps

[WPThroughput]_

Throttling vs. capping
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  
Bandwidth throttling works by limiting (throttling) the rate at which a bandwidth intensive device (a server) accepts data. If this limit is not in place, the device can overload its processing capacity.

Contrary to throttling, in order to use bandwidth when available, but prevent excess, each node in a proactive system should set an outgoing bandwidth cap that appropriately limits the total number of bytes sent per unit time.[8] There are two types of bandwidth capping. A standard cap limits the bitrate or speed of data transfer on a broadband Internet connection. Standard capping is used to prevent individuals from consuming the entire transmission capacity of the medium. A lowered cap reduces an individual user’s bandwidth cap as a defensive measure and/or as a punishment for heavy use of the medium’s bandwidth. Oftentimes this happens without notifying the user.

The difference is that bandwidth throttling regulates a bandwidth intensive device (such as a server) by limiting how much data that device can accept or receive. Bandwidth capping on the other hand limits the total transfer capacity, upstream or downstream, of data over a medium. 

[WPBandwidthThrottling]_