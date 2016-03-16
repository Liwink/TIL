Bug

1. match.start_at 没有判断 none 就和 datetime 比较。获取值时要兼容 none 的情况
2. set(none) 报错，先考虑为 none  的情况
3. 比赛没有分配统计员时，找不到统计员-比赛表，会对none做get，通过 or {} 可以避免
4. 比赛阵容，返回了两个队的阵容，看代码时发现了问题，但略过了。写单元测试时也忽略了混入其他球队的情况。改写时，自己写完了，但发现管爷已经改了。#拉去优先！ #问题随时记录！
5. 更新比赛事件后半个小时才想起来给客户端微信，但发现管爷已经在文档的 dictionary 中改好，通知客户端。 #规范化文档的维护 #即时沟通。
6. 转换 ObjectId，管爷默认在外层转换，可以使用 convert_to_object_id 装饰器进行转换。
7. 刷新统计员场数时，比赛可能没有 `start_at` 报错。 # None
8. 球员版统计数据的时候，需要筛选出正式统计员统计的数据。这个bug和比赛进球的bug一样，由于有测试统计员，一个事件可能被重复记录
9. supervisor 中 FATAL 的进程用 restart 重启
10. 报bug时难免会根据一次出现的场景来关联原因，比如提交审核被拒后，修改赛事信息后审核就通过了，上面的确是事实，但误导性。因为提交审核后，刷新页面也会显示提交成功。为了避免这些归因误差，了解实现原理是一个很重要的途径，知道原理就可以从底层推断出归因的不合理性。（但这种方式成本极高）另外则是不要轻易根据第一次发生的流程归因，尤其是「蹊跷」和复杂的过程。
11. jerkin中，test环境自动部署，staging会提示 public key 错误。排错，一是对比 test 和 staging 的配置，二是重点找和 ssh 有关的部分。最后错误也是出在没有设置 public。
12. 进行中的比赛没有刷新比赛阶段。我第一反应是：改过刷新比赛阶段的代码，第二反应是：改过config 代码。但管爷直接定位到 rq 和 消息队列。事实是管爷是正确的，也跟本质。我推测的依赖太多了。
13. 为什么刷新比赛慢。一是可能做了太多无用计算，二是 crontab 设定时间就是这样。
14. 用户中心可以正常登陆，Web版则提示认证错误。网页版使用的是 cookie 认证，但Web和用户中心的 config 中配置的 cookie key 不同。
15. git merge
16. 写 `utc_today` 时单独写为两个依赖的变量，获取值都为初始化值。应注意封装，后改为一个方法，这样更独立，不受影响。
17. if doc.get('playing_minutes') > 0  这种表达太错了，1. get后不能直接比较，因为没有类型判断，2. 这种逻辑判断，使用 if doc.get(‘playing_minutes’): 就足够了。
18.

	```
	def publish_by_list(channel, message):
	    subscribers = r.smembers(channel) or ()
    	message = utils.json_dumps(message)
    	for subscriber in subscribers:
        	channel = helpers.pack_subscriber_channel(channel, subscriber)
	        r.rpush(channel, message)
	```
	 这里会对 `channel` 重写，有多个 `subscriber` 时，就会出现 `[events][kvalue]event.add` 的情况。
