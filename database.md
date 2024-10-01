# Database

The database should be designed as an index of the storage objects, which primarily tracks their dependencies. Still in contrast to the current use-case in pyiron_base it should not keep track of the status of calculation, so the number of requests to this database should be reduced compared to the current database usage in pyiron_base. At the same time it should also represent the graph structure of the dependent objects / nodes, so most likely a second table is required.
