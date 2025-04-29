# Postgres Overview
## _My Notes On Postgres Features_

PostgreSQL is an object-relational database management system (ORDBMS) based on POSTGRES, Version 4.2, developed at the University of California at Berkeley Computer Science Department. It supports a large part of the SQL standard and offers many modern features. It's history dates back to 1986.

And because of the liberal license, PostgreSQL can be used, modified, and distributed by anyone free of charge for any purpose, be it private, commercial, or academic.
## Hierarchy
Tables are grouped into databases, and a collection of databases managed by a single PostgreSQL server instance constitutes a database cluster.

## Simple SQL Features
PostgreSQL supports an extended subset of the SQL standard, including transactions, foreign keys, subqueries, triggers, user-defined types and functions. This distribution also contains C language bindings.
- create table
- some advanced data types like point, json, etc.
- select,from,where syntax as usual
- join(inner, left outer, right outer and full outer)
- distinct, like, order by, having 
- aggregate(max, count, etc) , group by and having
- NEW 'filter' used together with selecting an aggregate
- updates, deletes
## Advanced SQL Features
- Transactions (Begin, Commit, Rollback) New: Savepoint Syntax "SAVEPOINT my_savepoint;"
- Window Functions: New: OVER (PARTITION BY city ORDER BY population)
- rank() to get the rank of a row in an order
- Inheritance to replace VIEWS(UNION of tables) because VIEWS get ugly with updates

POSTGRES supports a large part of the SQL standard and offers many modern features:
- complex queries
- foreign keys
- triggers
- updatable views
- transactional integrity
- multiversion concurrency control

Also, PostgreSQL can be extended by the user in many ways, for example by adding new
- data types
- functions
- operators
- aggregate functions
- index methods
- procedural languages
