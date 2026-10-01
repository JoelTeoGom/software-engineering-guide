<pre>
<b>DATABASE INTERNALS</b>
       <a href="https://www.uber.com/en-US/blog/postgres-to-mysql-migration/">Why Uber Engineering Switched from Postgres to MySQL</a>
              Learned how Postgres and MySQL (InnoDB) work internally: on-disk
              layout, index structure, MVCC, WAL vs redo log and replication,
              plus the trade-offs and use cases of each. The article where I've
              learned the most about database internals, by far.
       
       <a href="https://shopify.engineering/scaling-inventory-reservations">Shopify - We replaced Redis with MySQL for inventory reservations</a>
              How Shopify moved checkout reservations from Redis to MySQL to get ACID
              with the inventory ledger: one row per unit plus SELECT ... FOR UPDATE
              SKIP LOCKED, composite PKs to halve InnoDB row locks, READ COMMITTED
              to avoid gap locks and consistent lock ordering against deadlocks.
              Real bottleneck: connection hold time, not CPU or queries.
       
   <b>PostgreSQL</b>
       <a href="https://www.interdb.jp/pg/">The Internals of PostgreSQL - Hironobu Suzuki</a>
              Free online book and the best summary of how Postgres works inside:
              heap and tuple layout, MVCC (xmin/xmax), VACUUM, HOT updates,
              buffer manager, WAL and streaming replication.

       <a href="https://www.postgresql.org/docs/current/hot-standby.html">PostgreSQL - Hot Standby</a>
              How read replicas work and why long queries get cancelled
              ("Handling Query Conflicts"). Extends the replica MVCC problem
              described in the Uber article.

       <a href="https://www.postgresql.org/docs/current/wal-configuration.html">PostgreSQL - WAL Configuration</a>
              Checkpoints, WAL segments and full page writes (torn-page protection).

       <a href="https://www.postgresql.org/docs/current/storage.html">PostgreSQL - Database Physical Storage</a>
              How Postgres lays out data on disk: file layout, TOAST, free space
              map, visibility map and page layout.

   <b>MySQL (InnoDB)</b>
       <a href="https://dev.mysql.com/doc/refman/8.0/en/innodb-architecture.html">MySQL - InnoDB Architecture</a>
              Official diagram of InnoDB's in-memory and on-disk structures: buffer
              pool, change buffer, adaptive hash index, redo/undo logs and
              doublewrite buffer.

       <a href="https://blog.jcole.us/innodb/">Jeremy Cole - InnoDB internals</a>
              Byte-level deep dives into InnoDB's on-disk format: pages, records,
              B+tree indexes and page directory, with diagrams.

       <a href="https://dev.mysql.com/doc/refman/5.7/en/replication-formats.html">MySQL - Replication Formats</a>
              Statement-based vs row-based vs mixed binlog replication and their
              trade-offs. The logical replication Uber preferred over Postgres'
              physical WAL shipping.

   <b>Redis</b>
       <a href="https://redis.io/docs/latest/operate/oss_and_stack/management/optimization/benchmarks/">Redis Benchmark: Factors impacting Redis performance</a>
              Which factors actually drive Redis performance: network, single-
              threaded CPU, pipelining, NUMA and core pinning, VMs and slow fork,
              connection count and allocators. Key takeaway: Redis is usually
              network-bound before CPU-bound.

<b>SYSTEM DESIGN</b>
       <a href="https://discord.com/blog/how-discord-stores-trillions-of-messages">How Discord Stores Trillions of Messages</a>
              Learned how hot partitions happen even with a good partition key
              (channel + time bucket, Snowflake IDs), and why Cassandra's JVM GC
              pauses made it worse, leading to ScyllaDB. Key idea: protect the
              database with request coalescing plus consistent-hash routing by
              channel ID. When you need to squeeze every millisecond, the
              language matters: GC pauses in Java caused latency spikes that
              disappeared with C++ (ScyllaDB) and Rust (data services).

<b>BOOKS</b>
       <b>Database Internals</b> - Alex Petrov (Part I, ch. 1-5, 7)
              How storage engines work: pages, B-trees, splits, buffer management,
              WAL and recovery. Ch. 1 p.20 covers direct pointers vs primary-key
              indirection (the root of the Uber debate); ch. 7 covers LSM trees.

       <b>Designing Data-Intensive Applications</b> - Martin Kleppmann (ch. 3, 5, 7)
              Heap files vs clustered indexes (ch. 3), statement vs WAL vs logical
              replication (ch. 5), and how MVCC / snapshot isolation is
              implemented (ch. 7).

<b>AUTHOR</b>
       Joel Teodoro Gomez &lt;<a href="https://runtimerants.dev">runtimerants.dev</a>&gt;
</pre>
