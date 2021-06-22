Best practices and Tips

- [1. Pagination](#1-pagination)
  - [1.1. Don't paginate with offset](#11-dont-paginate-with-offset)
- [2. Pooling](#2-pooling)
- [SSL](#ssl)
# 1. Pagination
## 1.1. Don't paginate with offset
- offset by design is doing fetch all and drop first x rows.

# 2. Pooling
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


# SSL
- Open postgresql.conf and configure cert and key
- Generate cert and key and make it available for postgres
- 