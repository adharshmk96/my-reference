Partitioning & Sharding

- [1. Partitioning a table](#1-partitioning-a-table)
  - [1.1. Partitioning Type](#11-partitioning-type)
  - [1.2. Horizontal partition](#12-horizontal-partition)
    - [1.2.1. Automating Partitioning](#121-automating-partitioning)
  - [1.3. Vertical partition](#13-vertical-partition)
- [2. Sharding](#2-sharding)
  - [Consistant hashing ring](#consistant-hashing-ring)

# 1. Partitioning a table
- when a table gets large we can split into multiple parts.
- partitions are just more tables.


## 1.1. Partitioning Type
- Range : date, id
- List : discreet values (state, zipcode)
- Hash : hash functions (consistent hashing)

## 1.2. Horizontal partition
- Splitting rows into partition tables in same database

eg: manual method
```
-- Create a table

CREATE TABLE table_name (id serial not null, g int not null) partition by range (g)

-- Create Partition tables

CREATE TABLE table_35_60 (like table_name including indexes);
CREATE TABLE table_60_80 (like table_name including indexes);

-- attach partition

ALTER TABLE table_name ATTACH PARTITION table_00_35 for values from (0) to (35);
ALTER TABLE table_name ATTACH PARTITION table_35_60 for values from (35) to (60);

-- Auto inserts to corresponding partition when inserting to main tb

insert into table_name(g) select floor(random()*100) from generate_series(0,10000000);

-- Auto generates indexes for paritions when created on main tb

create index table_name_idx on table_name(g);

```
```
-- ANALYZE SIZE

select (pg_relation_size(oid) / (1000*1000)), relname from pg_class order by pg_relation_size(oid) desc;
```
- Improves performance when accessing single partition
- Sequential scan vs scattered index scan
- Easy bulk loading (attaching partition )
- Archive old data will get better.

### 1.2.1. Automating Partitioning
- Use a programming language and connect to db and loop create table
- The tables has to be created individually and attached to main table

```
for(i=0;i<100;i++) {
    pg.query(`
        CREATE TABLE table_${i}_${i} (like table_name including indexes);
    `)

    pg.query(`
        ALTER TABLE table_name ATTACH PARTITION table_${i}_${i} for values from (${i}) to (${i});
    `)
}
```


## 1.3. Vertical partition
- Splitting columns (when size is large and query result is slowing down)

# 2. Sharding
- Splits table into multiple spread across different dbs (servers / instances)

## Consistant hashing ring
- Get a more defined or consistant hashing to switch between shards
