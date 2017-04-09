---
layout: post-talk-jsconf13
title: Selena Deckelmann - PLV8
category: JSConf US 2013
youtubeID: wn2SpeHhWVc
---

Schema design & JSON didn't get on before plv8 and JSON datatype

Schema liberation:
modelling documents/schemas so they closely resemble application-level objects
(MongoDB has flexible schema for this reason so it's popular with developers)

JSON far more friendly to the developer than a set of SQL statements

Can Postgres be flexible?

9.3 Postgres + plv8
(9.2 postgres + json\_enhancements works too)

v8 = js engine
plv8 = v8 embedded in Postgres

```sql
    select * from reports;
    select row_to_json(*) from reports (doesn't work - need subselect to achieve it)
    create table liberated as (
      // subselect here
    );
```

2 mins to run this on huge db

[https://gist.github.com/selenamarie/5646494](https://gist.github.com/selenamarie/5646494)

JSON datatype is a First class datatype

```sql
    create table birds (sighting json);
    insert into birds values (
      // json object here
    );
    INSERT 0 1;
```

create indexes, compare, join against, use JSON columns like any other Postgres
column, etc.

plv8 trusted - don't need to be a superuser to run (python is untrusted for
instance, need to be superuser) - plv8 can be run by users

supports running raw SQL, prepared statements, cursors

direct JS injection (if you're crazy enough to want to...!)

why use postgres to store JSON vs NoSQL DB?
1. use "bulkbag" schema design + schema evolution - JSON to start, normalize to
optimize. I.e. store the data at first, work out best way to normalize it later
once true structure known·

2. easily scale to multi-TB DBs, write/read heavy loads, non-cloud storage, etc

3. manage your data with a language you love

9.3 beta is available now - postgresql.org/developer/beta

Twitter: [@selenamarie](http://twitter.com/selenamarie)

