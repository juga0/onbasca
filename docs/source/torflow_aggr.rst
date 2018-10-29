.. _torflow_aggr:

Torflow measurements aggregation
==================================

Scaling without PID
--------------------

From Torflow's README.spec.txt (section 2.2)

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 315, 316, 317

The variables and steps used in Torflow:

**strm_bw**

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 264, 265

::

    strm_bw = sum(bw stream x)/|n stream|

**filt_bw**

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 267-269

**filt_sbw and strm_sbw**

.. literalinclude:: ../../docs_torflow/SQLSupport.py
   :lines: 495-514

**filt_avg, and strm_avg**

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 305, 306

.. literalinclude:: ../../docs_torflow/aggregate.py
   :lines: 519, 520

**true_filt_avg and true_strm_avg**

.. literalinclude:: ../../docs_torflow/aggregate.py
   :lines: 537-539

In the non-pid case, all types of nodes get the same avg

**n.fbw_ratio and n.fsw_ratio**

.. literalinclude:: ../../docs_torflow/aggregate.py
   :lines: 585-587

**n.ratio**

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 308, 309

.. literalinclude:: ../../docs_torflow/aggregate.py
   :lines: 742-764

**desc_bw**

It is the ``observed bandwidth`` in the descriptor, NOT the ``average
bandwidth``

.. literalinclude:: ../../docs_torflow/TorCtl.py
   :lines: 489-491

.. literalinclude:: ../../docs_torflow/TorCtl.py
   :lines: 384

**new_bw**

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 311-313

.. literalinclude:: ../../docs_torflow/aggregate.py
   :lines: 750

The descriptor observed bandwidth is multiplied by the ratio.

**Limit the bandwidth to a maximum**

.. literalinclude:: ../../docs_torflow/aggregate.py
   :lines: 44

.. literalinclude:: ../../docs_torflow/aggregate.py
   :lines: 778-784

However, tot_net_bw does not seems to be updated when not using pid.
This clipping would make faster relays to all have the same value.

All of that can be expressed as:

.. math::

    bwn_i &=
        max\left(
            \frac{bw_i}{\mu},
            \frac{bwf_i}{\mu_{bwf}}
            \right)
        \times bwobs_i

.. math::

     bwn_i &=
        max\left(
            \frac{bw_i}{\mu},
            min \left(
                bw_i,
                bw_i \times \mu
                \right)
                    \times
                    \frac{bw_i}{\sum_{i=1}^{n}
                    min \left(bw_i,
                        bw_i \times \mu
                    \right)}
            \right)
        \times bwobs_i \

     &=
        max\left(
            \frac{bw_i}{\frac{\sum_{i=1}^{n}bw_i}{n}},
            min \left(
                bw_i,
                bw_i \times \frac{\sum_{i=1}^{n}bw_i}{n}
                \right)
                    \times
                    \frac{bw_i}{\sum_{i=1}^{n}
                    min \left(bw_i,
                        bw_i \times \frac{\sum_{i=1}^{n}bw_i}{n}
                    \right)}
            \right)
        \times bwobs_i



Torflow PID Control feedback
-----------------------------

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 371,372

(=`filt_bw`?)

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 374,375

(=`filt_avg`?)

do we really want SP to be F_avg?

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 377-379

::

    bwf = filt_bw

.. math::

    e(t) = \frac{bwf_i}{\mu_{bwf}} - 1

of the entire population or the sample of this measurement?

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 381-389

::

    new_consensus_bw = bwwn
    old_consensus_bw = cbw_o

.. math::

    bwwn = bwwo +
            bwwo K_p e(t) +
            bwwo K_i \int _{0}^{t}{e(t)} +
            bwwo K_d \frac {d}{dt}{e(t)}

why the 1st term?

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 391, 398

what is `new_bw` and `old_bw` here?

.. math::
	K_p = 1, K_i=K_d=0: \\
  bwn =& bwo + bwo e(t) \\
  bwn =& bwo + bwo \left(\frac{bwf_i}{\mu_{bwf}} - 1\right) \\
  bwn =& bwo \frac{bwf_i}{\mu_{bwf}}

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 306-313

::

    bw_i = strm_bw

In the scaling without PID controller

.. math::

    bwn_i =&
        max\left(
            \frac{bw_i}{\mu},
            \frac{bwf_i}{\mu_{bwf}}
            \right)
        \times bwobs_i

So the called ratio is the same, but not what is considered the new bw.

.. literalinclude:: ../../docs_torflow/README.spec.txt
   :lines: 421-431

For the integral component: how the decay factor works?

.. math::

    K_i \int _{0}^{t}{e(t)} = ?

For the differential component:

.. math::

	K_d \frac {d}{dt}{e(t)} = e(t_{i-1}) - e(t_{i})