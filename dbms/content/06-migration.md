Migrations in DB

- [1. Schema migration](#1-schema-migration)
- [2. Data migration](#2-data-migration)

# 1. Schema migration
- Migration or change of DB table structure 
- Plain sql queries (DDL) that defines or alters table
- Must be written into versioned files with corresponding reverting query
  - migrate up should migrate changes
  - migrate down should revert the change made

# 2. Data migration
- Transformation or Moving the data values itself
- It can be within a table or across database
- more critical, have to programattically handle the issue