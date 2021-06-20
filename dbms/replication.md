Replication in postgres

- Horizontal Scaling
- Region based queries
- Evential consistency
- slow writes in sync
- complex for multi master

# Master - Standby
- Central master node that accepts writes / ddls
- Multiple Standby nodes that receive the writes
- Simple, no conflict

# Master - Master
- Multiple master nodes hat accepts writes / ddls
- Mutiple standby nodes that receive the writes
- Conflict resolution is needed.

# Sync and Async
## Sync 
- A write transaction is considered incomplete until written to all standby nodes.
- Wait until option ( wait until first 2 replicas are synced )

## Async
- A write transaction is considered complete if its written to master.
- Write is propogated async to the standby nodes.

## Replication Steps
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

