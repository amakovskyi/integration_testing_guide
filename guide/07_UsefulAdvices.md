# Useful advices

## Pooling

Sometimes test may require a long-time setup because of filling a lot of data.

_For example, user profile in social network can contain number of posts, likes, followers, etc. To fill it is needed a
lot of time to create several users, cross-follow, create posts and other stuff. It is a long-running operation._

If such data can be reused between not just test from one file - use pooling. Create separate file, which holds pool of
required items (ids, full data, logged-in user ApiClients etc.) and on first retrieve-invocation build data, but on all
next invocation just return cached data.

This technique can speed up some tests dramatically.

## Standard tests

### Pagination testing

Testing ```offset``` or other pagination parameters, use next pattern:

* retrieve list with 10 items from start as ```list```
* retrieve list with 5 items from after 5 first items as ```listOffset```
* validate items from ```listOffset[0..4]``` has same ids as items from ```list[5..9]```

### Counters testing

Testing ```total``` counter:

* retrieve total counter as ```totalBefore```
* create/delete/block/etc. ```x``` items into counted list
* retrieve total counter as ```totalAfter```
* validate ```totalBefore + x == totalAfter```

Do the same fo counters based on any selection filters.