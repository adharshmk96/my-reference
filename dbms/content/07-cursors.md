Database Cursors.

- Used for very large data sets (millions of rows)
- Cursors allow us to seek row by row on a large dataset.

## Declaring cursor 
- Declaring a cursor won't execute the query, it just prepares the plan
  
```
begin
declare c cursor for select col from table where col between x and y

```
## Fetching

- Fetching happens when we actually use keyword fetch
```
FETCH FIRST FROM c;
FETCH LAST FROM c;
FETCH NEXT FROM c;
FETCH PRIOR FROM c;
FETCH ABSOLUTE 7 FROM c;
FETCH RELATIVE -2 FROM c;
```

- Fetch will execute relative to its current position.
  
## Closing
- To close the connection
```
CLOSE c;
```

## Deallocate
- To deallocate cursor memory
```
DEALLOCATE C;
```
- Transaction end will auto deallocate cursor


## Client side cursor
- Cursor initiated from client, bulk data transfer happens.

eg:
```
cur = connection.cursor()

cur.execute("select * from table")
-- Fetches all row into the cursor memory in client.

rows = cur.fetchmany(50)
-- gets values from already loaded data in client.

cur.close()
```
- High bandwidth, Data loaded more on client, less overhead on server.


## Server side cursor
- Cursor is created on db and only gives value up on needed

eg:
```
cur = connection.cursor("cursor-name)

cur.execute("select * from table")
-- Fetches all row into the cursor memory in server.

rows = cur.fetchmany(50)
-- gets values from the server by seeking.

cur.close()
```
- Less bandwidth, Data loaded only in server, more overhead on server.
- Cursors left open, leaking cursors etc..



- Saves Memory especially in client side.
- Streaming is possible with cursors.
- Cursors can be cancelled.

- Cursors are stateful
- Transaction will be running for long time (ddl and indexing are halted)