.. _requirements:

Requirements (Mar 2018)
========================

Based on [BwAuthRome]_

Must (short term)
-----------------

- easy to run
- easy to maintain
- not too expensive to run
- fail early
- output:

  - meanful diagnostic outputs
  - write results as soon as they are produced instead of wait until all relays are acanned
  - bandwidth output: one integer per tor relay
  - if it takes a long time to run, run it on a subset of the network so we can look at the results
  - numbers can be different to ``torflow``, but format should be the same

- input:

  - include new relays within a short time: prefer to scan volatile relays;
  - if a relay has been stable, deprioritize it
  - not rely on self-reported capacity, though it can start with those reports

- network interaction:

  - not slow down the network
  - multiple "pipes of measurement" must be able to coexist
  - be able to run multiple scanners at once
  - if we run multiple bwscanners concurrently then their output measurements need to be compatible -- normalize measurements so that they can be comparable.
  - dirauths might need to normalize values between different implementations
  - bwauths?: combination is _median_; add up all the numbers and divide by N is a plausible way to normalise the numbers
  - [?] self-reported measurements should be treated as a cap, so that we don't overallocate traffic through that.
  - must not reduce network diversity by pushing out slow relays: while there is not feedback mechanism, this won't happen?

- privacy

  - it should not measure in such detail that it is effectively a global adversary.

- algorithm:

  - measure by time, not by size. fetch over a certain amount of time, throw away the first and last bits, see how much went through
  - have script that builds path and fetches data should be separate from decisions about parameters of what was fetched and for how long.
  - express each weight as a proportion of the total, and multiply by some agreed total (e.g. for the current network it would have to be the total of the consensus weight, but within some limited range to avoid unbounded growth)
    ie: relays_bw = [10, 70, 100], total_bw = sum([10, 70, 100]) = 180, relays_bw_normalized = [1/18, 7/18, 10/18]
  - be able to normalize measurements.
  - educated guesses, not making a robustly tested experiment.

- deployment:

  - needs to be deployable to ~5 people, not to hundreds

- milestones:

  - should be a script that produces numbers
  - should be run on the real network asap
  - 2/6/8 months short term

Nice to have (middle term)
---------------------------

- network interaction:

  - avoid committing to a full batch -- if the scanner is looping, at the top of each loop it could identify specific relays that you want to test.
  - all load to all the computers in one country is not so good
  - spreads load over the world to get our distributed privacy properties.
  - resolution and volatility should not be greater than necessary. relative stability makes consensus diffs easier to compress.

- security:

  - threat model in terms of making it gamable
  - bandwidth authorities themselves must not be detectable so that they can't be gamed.

- measure cpu load, memory, socket availability, ram preasure, etc.
- run tests/experiments with shadow
- scripts that show the differences between bw authority data (tom ritter)
- simple tool that can be re-used for bridge measurement

Long term
----------

- we want to measure between pairs of relays, not specific relays
- not actually just measuring bandwidth; it's a mix of latency, diversity, etc. measurements from australia will give different results from measurements from austria.
- client-side changes should be explicitly separated from measuring relays.
- if we don't max out our slow relay operators, then we might be discouraging them from increasing capacity.
- "proof of storage" protocol, which delegates the bandwidth measurements to little relays, which run in aggregate to measure bigger relays
