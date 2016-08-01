## MongoDB 实战



> 这种将数据拆分到多张表里的技术成为正规化（**normalization**）。正规化的数据希望保证每个数据单元仅出现在一个地方。
>
> MongoDB 把文档组织成集合，这种容器无序任何类型的 Schema。

##### 即时查询（ad hoc query）

> *Ad hoc* is latin for "for this purpose". 

即时查询：无需预先定义系统接受的查询类型？

- 预先是针对什么？
- 查询类型？

```sql
SELECT * FROM posts
INNER JOIN posts_tags ON posts.id = posts_tags.post_id
INNER JOIN tags ON posts_tags.tag_id == tags.id
WHERE tags.text = 'politics' AND posts.vote_count > 10;
```

```javascript
db.posts.find({'tags': 'politics', 'vote_count': {'$gt': 10}})
```

MongoDB 不支持即时查询，但提供类似的查询能力。

##### 复制

MongoDB 通过**副本集**（replica set）的拓扑结构提供了复制功能。副本集将数据分布在多台机器上以实现冗余，在服务器和网络故障时能提供自动故障转移。

副本集由一个主节点（primary node）和若干个从节点（secondary node）构成。副本集的主节可以接受读写操作，但从节点是只读的。同时副本集可以支持自动故障转移。

#### 索引

```javascript
> db.data_events.find( {code: 71} ).explain("executionStats")
{
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 13923,
		"executionTimeMillis" : 7588,
		"totalKeysExamined" : 0,
		"totalDocsExamined" : 2429488   #!!!
        # ...
	}
}

```

建立索引

```javascript
> db.data_events.ensureIndex({code: 1})
> db.data_events.find( {code: 71} ).explain("executionStats")
{
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 13923,
		"executionTimeMillis" : 29,
		"totalKeysExamined" : 13923,
		"totalDocsExamined" : 13923   # !!!!!
        #...
	}
}
```

#### Schema 设计

- 数据的基本单元是什么？
  在 RDBMS 中有带列和行的数据表。在 MongoDB 中，数据的基本单元是 BSON 文档。
- 如何查询并更新数据？
  RDBMS 有即时查询和联结操作。MongoDB 也有即时查询，但不支持联结操作。
  **RDBMS 将多个更新封装在一个事务中，以获得原子性，还能回滚**。MongoDB 不支持事务，但它支持多种原子更新操作。
- 应用程序的访问模式是什么？
  读写比是多少？需要何种查询？数据是如何更新的？能想到什么并发问题？数据的结构化程度如何？

#### 更新

MongoDB  的更新有 **替换** 和 **原子针对性更新** 两种。

从 oplog 中可以看出：

`$set` 更新射正时的 oplog

```json
{
    "ts" : Timestamp(6307516012296142, 6),
    "h" : NumberLong(-222767677461708999),
    "v" : 2,
    "op" : "u",
    "ns" : "opstaging.stats_player_match",
    "o2" : {
        "_id" : ObjectId("578890bf599b242827692e00")
    },
    "o" : {
        "$set" : {
            "shot_on_target" : 1
        }
    }
}
```

MongoKit 中的 save 是默认替换，下面是更新球员上场时间的 oplog

```json
{
    "ts" : Timestamp(6307516115375358, 101),
    "h" : NumberLong(-7301462391673997344),
    "v" : 2,
    "op" : "u",
    "ns" : "opstaging.stats_player_match",
    "o2" : {
        "_id" : ObjectId("578890bf599b242827692e00")
    },
    "o" : {
        "_id" : ObjectId("578890bf599b242827692e00"),
        "key_pass" : 0,
        "created_by" : null,
        "attend_match_count" : 0,
        "unsuccessful_take_on" : 0,
        "goal" : 0,
        "rating" : 5.96969658730226,
        "block" : 0,
        "throw_in" : 0,
        "foul_conceded" : 0,
        "playing_minutes" : 1.18333333333333,
        "challenge_lost" : 0,
        "competition_id" : null,
        "successful_take_on" : 0,
        "goal_assist" : 0,
        "updated_by" : null,
        "updated_at" : ISODate("2016-07-15T11:43:57.741Z"),
        "player_id" : ObjectId("56f412f9599b242d0eecf953"),
        "roam_to" : 1,
        "rating_real" : 5.96969658730226,
        "blocked" : 0,
        "team_id" : ObjectId("56f41378599b242d0eecf961"),
        "total_match_count" : 0,
        "match_start_at" : ISODate("2016-07-15T10:00:00.000Z"),
        "own_goal" : 0,
        "shot_on_target" : 1,
        "penalty" : 0,
        "free_kick" : 0,
        "match_end_at" : null,
        "round_id" : null,
        "match_id" : ObjectId("5788906a599b2458ca93c336"),
        "season_id" : null,
        "successful_pass" : 0,
        "year" : 2016,
        "woodwork" : 0,
        "yellow_card" : 0,
        "match_style" : "free",
        "group_id" : null,
        "shot_off_target" : 0,
        "red_card" : 0,
        "tackle_lost" : 0,
        "clearance" : 0,
        "status" : null,
        "offside" : 0,
        "created_at" : ISODate("2016-07-15T07:29:03.684Z"),
        "tackle_won" : 0,
        "challenge_won" : 0,
        "motm" : 0,
        "corner" : 0,
        "interception" : 0,
        "save" : 0,
        "unsuccessful_pass" : 0,
        "stats_mode" : 0
    }
}
```

为什么 `v` 都是 2？