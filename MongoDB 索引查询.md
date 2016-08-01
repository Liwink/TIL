## MongoDB 索引查询

#### 查询

index

```json
[
	{
		"_id" : 1
	},
	{
		"match_id" : 1
	},
	{
		"team_id" : 1
	},
	{
		"player_id" : 1
	},
	{
		"client_event_id" : 1
	}
]
```

query

```json
{
	"match_id" : ObjectId("5655233cc8f97c116eaac685"),
	"role" : "statistician",
	"code" : {
		"$in" : [
			101,
			102,
			103,
			104,
			105,
			106,
			107,
			108
		]
	}
}
```

sort

```json
{ "client_started_at" : 1 }
```

explain

```json
>  db.data_events.find(q).explain("executionStats").executionStats.executionTimeMillis
68
> db.data_events.find(q).explain("executionStats").executionStats.totalDocsExamined
45296
```

#### 建立 code 索引

```javascript
db.data_events.ensureIndex({"code": 1}, {"background": 1})
```

explain

```json
> db.data_events.find(q).sort(s).explain("executionStats").executionStats.executionTimeMillis
37
> db.data_events.find(q).explain("executionStats").executionStats.totalDocsExamined
8155
```

#### 建立 client_started_at 索引

```javascript
db.data_events.ensureIndex({"client_started_at": 1}, {"background": 1})
```

explain

```json
> db.data_events.find(q).sort(s).explain("executionStats").executionStats.executionTimeMillis
50
> db.data_events.find(q).sort(s).explain("executionStats").executionStats.totalDocsExamined
8155
```

#### 建立 code_role 索引 （删 code 索引

```javascript
db.data_events.dropIndex({"code":1})
db.data_events.ensureIndex({"code": 1, "role": 1}, {"background": 1})
```

explain

```javascript
> db.data_events.find(q).sort(s).explain("executionStats").executionStats.executionTimeMillis
28
> db.data_events.find(q).sort(s).explain("executionStats").executionStats.totalDocsExamined
3921
```

#### 建立 role_code 索引

```javascript
db.data_events.dropIndex({"code": 1, "role": 1})
db.data_events.ensureIndex({"role": 1, "code": 1}, {"background": 1})
```

explain

```javascript
>  db.data_events.find(q).sort(s).explain("executionStats").executionStats.totalDocsExamined
3921
>  db.data_events.find(q).sort(s).explain("executionStats").executionStats.executionTimeMillis
26
```

在这一例中，两个索引顺序没有影响，但是后者更合理。



---

#### Reference:

[Analyze Query Performance](https://docs.mongodb.com/manual/tutorial/analyze-query-plan/)