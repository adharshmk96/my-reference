Best practices and Tips

# Pagination
## Don't paginate with offset
- offset by design is doing fetch all and drop first x rows.

# Pooling
- Used when connection est, tear down are slow.
- Create pool of connection
- Uses a connection and executes query

eg: 
```
const pool = new Pool({
        host: "pg1",
        port: "5432",
        user: "postgres",
        password: "postgres",
        database: "postgres",
        max: 20,
        connectionTimeoutMillis: 0,
        idleTimeoutMillis: 0
    })


await pool.query("...")
```