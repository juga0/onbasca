BwScanner in detail
=====================

`bwscanner code <https://github.com/TheTorProject/bwscanner>`_


Path algorithm
---------------

- Create set with exits
- Order exits by bw
- Take relay subset as random from 0? to num total relays (including exits?) taken by number of partitions
- For every relay in a sample of the relay subset

  - Build path with that relay

    - For every exit

      - If exit bandwith < relay bandwith and there're still exits

        - Take other exit

      - Take exits slice from the exit we are to the nuber of exits slice
      - Take exits needed as the number of exits slice - the number of exits slice
      - If there're exits needed

        - ...

      - If relay is in exits slice

        ...

      - Return a random from exit slice
