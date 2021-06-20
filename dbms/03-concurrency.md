Concurrency Control

- [1. Locks](#1-locks)
  - [1.1. Exclusive Lock](#11-exclusive-lock)
  - [1.2. Shared Lock](#12-shared-lock)
  - [1.3. Deadlock](#13-deadlock)
  - [1.4. Row level lock in postgresql](#14-row-level-lock-in-postgresql)
  - [1.5. Two Phase Locking](#15-two-phase-locking)
    - [1.5.1. Double booking problem (Race condition)](#151-double-booking-problem-race-condition)

# 1. Locks
- Used to ensure consistancy

```
LOCK [ TABLE ]
name
 IN
lock_mode

lock_mode { ACCESS SHARE, ROW SHARE, ROW EXCLUSIVE, SHARE UPDATE EXCLUSIVE, SHARE, SHARE ROW EXCLUSIVE, EXCLUSIVE, ACCESS EXCLUSIVE }
```

- Locks are released at the end of txn

## 1.1. Exclusive Lock
- Write lock, no one else can read / write on resouce with exclusive lock

## 1.2. Shared Lock
- Read lock, no one else can write (allowed to read) on resource with shared lock
- When a value is being read and it needs to stay same through out.

## 1.3. Deadlock
- When multiple transaction is waiting on eachother to proceed.

eg: consider two transactions on table with field id only

T1
```
BEGIN

insert into table values(20);

insert into table values(21);
```

T2
```
BEGIN

insert into table values(21);

insert into table values(20);
```

Deadlock will be detected and one transaction will be rolledback.

## 1.4. Row level lock in postgresql
`select * from seats where id = 14 for update;`

## 1.5. Two Phase Locking
1. Growing phase
   - New locks can be obtained.
2. Shrinking Phase
   - Existing locks can be released but new locks can't be acquired. 

### 1.5.1. Double booking problem (Race condition)
- When two transaction reads same info and tries to update same row.

EG: booking table with id, isbooked, name

T1
```
BEGIN

select * from seats where id = 14;

update seats set isbooked = 1, name = 'T1' where id = 13;

```

T1 successfully updates it.

T2
```
BEGIN

select * from seats where id = 14;

update seats set isbooked = 1, name = 'T1' where id = 13;

```

T2 will be waiting for T1 to be completed

T1
```
commit;
```

DB will be updated.

T2
```
commit;
```

Update from T1 will be lost and T2's update will overwrite T1.

Solution : 2 phase locking.

```
BEGIN

select * from seats where id = 14 for update; (growing, any t can acquire)


```