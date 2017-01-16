# Presto

This is a project to learn about [Presto](https://prestodb.io/), it is an open source distributed SQL query engine for running interactive analytic queries against data sources of all sizes ranging from gigabytes to petabytes.

Presto was designed and written from the ground up for interactive analytics and approaches the speed of commercial data warehouses while scaling to the size of organizations like Facebook.

## Setup

This writeup assumes that there is already a set of polyglut databases and they have been populated with data, here we are focused more on the Presto setup. This writeup works with Postgres and Cassandra, [although you could have other connectors](https://prestodb.io/docs/current/connector.html).

## Installing Presto Server for Mac

The easiest way to install Presto is with [Homebrew](http://brew.sh).

```sh
brew install presto
```

Next, add a connector. Here’s the list of [available ones](https://prestodb.io/docs/current/connector.html).

For PostgreSQL, create `/usr/local/Cellar/presto/0.150/libexec/etc/catalog/postgres.properties` with:

```ini
connector.name=postgresql
connection-url=jdbc:postgresql://localhost:5432/postgres
connection-user=seizadi
connection-password=
```

And start the server with:

```sh
presto-server run
```

## Client

Presto comes with a CLI

```sh
presto --catalog postgres --schema public
```

And run:

```sql
show tables;
```

Try one of your tables with:

```sql
select * from <some_table>;
```

You can do more advanced queries:
```sql
presto:public> select * from users where email='customer1dev@infoblox.com';
 id |     name     | role | tenant_id  | approved |            auth_token            |           email           |                      encrypted_password                      | r
----+--------------+------+------------+----------+----------------------------------+---------------------------+--------------------------------------------------------------+--
 41 | Customer Dev |    1 |         23 | true     | a420f3cc7609495abcd2e4bf989925dd | customer1dev@infoblox.com | $2a$10$rzXabclq8IyxHmEF9zBaa.h8RYKDBEt.3cjKSF3BgEkhvB4lmlQ42 | ....
```

If the use case is to expose the base Presto connector to end users it will not work for multi-tenancy. As you see the Presto SQL client gets full access to the underlying Table and will see data that is segmented by tenant_id.

The [postgres connector implementation is found in Presto Github](https://github.com/prestodb/presto/tree/master/presto-postgresql).

### Client Libraries
There are also clients in [many different languages](https://prestodb.io/resources.html#libraries).

## References
[Apache Presto Tutorial](https://www.tutorialspoint.com/apache_presto/apache_presto_quick_guide.htm)

