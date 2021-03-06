Replication in postgres

- Horizontal Scaling
- Region based queries
- Evential consistency
- slow writes in sync
- complex for multi master

- [1. Master - Standby](#1-master---standby)
- [2. Master - Master](#2-master---master)
- [3. Sync and Async](#3-sync-and-async)
  - [3.1. Sync](#31-sync)
  - [3.2. Async](#32-async)
  - [3.3. Replication Steps](#33-replication-steps)
# 1. Master - Standby
- Central master node that accepts writes / ddls
- Multiple Standby nodes that receive the writes
- Simple, no conflict

# 2. Master - Master
- Multiple master nodes hat accepts writes / ddls
- Mutiple standby nodes that receive the writes
- Conflict resolution is needed.

# 3. Sync and Async
## 3.1. Sync 
- A write transaction is considered incomplete until written to all standby nodes.
- Wait until option ( wait until first 2 replicas are synced )

## 3.2. Async
- A write transaction is considered complete if its written to master.
- Write is propogated async to the standby nodes.

## 3.3. Replication Steps
- Create identical instances (same files, or bkp and write)
- add replication with a proper user role (not admin)
- edit **pg_hba.conf** of master file system
  - host replication <master_replication_user> all md5
- edit **postgresql.conf** of standby file system
  - primary_conninfo = 'application_name=standby1 host=<masterhost> port=<masterport> user=<master_replication_user> password=<master_replication_user_pwd>'
- add new file **standby.signal** to standby file system
- edit **postgresql.conf** of master file system
  - synchronous_standby_names = 'first <n> (standby1, standby2, ..., standby<m>)'
  - where n is the first n priority writes

