Indexing & Caching

HeapFile : binary files in which data is stored.

Page (blocks) - heap file divided into pages with a number of tuples.
[Ref](https://www.postgresql.org/docs/13/storage-page-layout.html)

Tuple - Individual rows from table.

- [1. Primary Index](#1-primary-index)
- [2. Secondary Index](#2-secondary-index)
- [3. Primary key Default Index](#3-primary-key-default-index)
- [4. Indexing](#4-indexing)
  - [4.1. Specify which column to index](#41-specify-which-column-to-index)
- [5. Scans](#5-scans)
  - [5.1. Index scan](#51-index-scan)
  - [5.2. Index only scan](#52-index-only-scan)
  - [5.3. Sequential scan](#53-sequential-scan)
  - [5.4. Bitmap Scan](#54-bitmap-scan)
  - [Index vs Bitmap scan](#index-vs-bitmap-scan)
- [6. Caching](#6-caching)

**Fragmentation is more important in performance**

# 1. Primary Index
- Arrangement of tuples in intelligent way based on a primary key order.
- index oriented table

# 2. Secondary Index
- A second index (mostly btree, lsm tree) where location information (page and tuple inside heap) for a key is stored.

*PG has secondary index only.*

# 3. Primary key Default Index
- Every primary key is indexed by default in postgres (as secodary index).

# 4. Indexing 
## 4.1. Specify which column to index

key, non-key index

`create index index_name_idx on table(col) include(col2);`

composite index

`create index index_name_idx on table(col, col2);`
- can't use index on col2 filtering, pg only uses lhs col
- make another index for col2 if we do col2 filtering.
- this is useful for col = val and col2 = val2 lookups

concurrently 

`create index concurrently index_name_idx on table(col);`

An index will be generated with mapping of heap. (avoid blocking production db operations)

# 5. Scans
## 5.1. Index scan
- Scans the index for the key and fetch data from corresponding heap.
- index created with `create index index_name_idx on table(id);`
- queried with `select col2 from table where id = {val}`

## 5.2. Index only scan
- Scans only the index to get all values needed for query.
- index created with `create index index_name_idx on table(col) include(col2);`
- queried with `select col2 from table where col = {val}`

## 5.3. Sequential scan
- Sequentially scan all pointers of all pages of the table.
- If no index is available, this is executed (during select *)

## 5.4. Bitmap Scan
- Scan index page once, generate a bitmap for the heap and then fetch data from heap.
- Slower for some non-key
- index created with `create index index_name_idx on table(col);`
- queried with `select col2 from table where col = {val}`

## Index vs Bitmap scan  
- index scan jumps to heap scan and index scan back and forth.
- bitmap scan builds a bitmap and then executes bitmap-heap scan for all the pages.

# 6. Caching
- After querying, DB Caches the result for sequential fetches.

Examples

- select id from table where id = {id} 
  - index only, Fastest
- select name from table where id = {id} 
  - index and disk look up, Slower but cached (faster next time)
- select id from table where col = {value} 
  - sequential disk look up, slower
- select id from table where col like %{value}% 
  - sequential disk look up and operation, slowest

eg: table with cols id, g, name

index can get slower on non key cols
- select name from grade where g = 30; -> 1800ms (table lookup)
- create index g_idx on grade(g);
- select name from grade where g = 30; -> 2100ms (index scan + table lookup)

append needed val with non key cols for faster query
- select name from grade where g = 30; -> 1800ms (table lookup)
- create index g_idx on grade(g) include name;
- select name from grade where g = 30; -> almost 1800ms (index only scan)