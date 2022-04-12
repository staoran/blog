---
title: LowDB 使用文档
date: 2022-04-12 22:53:48
updated: 2022-04-12 23:07:45
categories:
  - [学习笔记, 前端备忘]
tags:
  - 前端
  - LowDB
  - 数据持久化
---

> lowdb 是基于[lodash](https://lodash.com/docs)构建的，因此我们可以使用任何 lodash 强大的函数。并且我们可以串联使用。

```typescript
// 引入 LowDB
// 遇到引用问题请降级，使用 1.0.0 或 2.X.X 再试
const lowdb = require("lowdb");
const FileSync = require("lowdb/adapters/FileSync");
// const lowdb = import('lowdb')
// const FileSync = import('lowdb/adapters/FileSync')
// import lowdb from 'lowdb'
// import FileSync from 'lowdb/adapters/FileSync'
const adapter = new FileSync("./db.json");
const db = lowdb(adapter);

// 设置默认数据结构 (如果你的 JSON 文件为空)
// 推荐设置，不设置的话如果是新的 json 文件,或者变更了结构,会报错
db.defaults({ servers: [] }).write();

console.log(`清理过期缓存`);
// (p) => p.activityEndTime < new Date().getTime() 高级查询写法

// 批量删除
db.get("servers")
  .remove((p) => p.activityEndTime < new Date().getTime())
  .write();

// 查询多条
var value = db
  .get("servers")
  .filter((p) => p.activityEndTime < nowTime.getTime())
  .value();

// 查询单条
var value = db
  .get("servers")
  .find((p) => p.activityEndTime === nowTime.getTime())
  .value();

// 另一种匹配方式，单条
db.get("servers").filter({ activityEndTime: nowTime.getTime() }).value();
db.get("servers").find({ activityEndTime: nowTime.getTime() }).value();

// 排序和取前X条
db.get("servers")
  .filter({ activityEndTime: nowTime.getTime() })
  .sortBy("activityEndTime")
  .take(5)
  .value();

// 取数量
db.get("posts").size().value();

// 取指定行的指定列数据
db.get("posts[0].title").value();

// 查询并更新，assign方法能找到列字段则更新，找不到则新增列并写入
db.get("posts").find({ title: "low!" }).assign({ title: "hi!" }).write();

// 删除一行
db.get("posts").remove({ title: "low!" }).write();

// 删除属性（列）
db.unset("user.name").write();
// 深度克隆
db.get("posts").cloneDeep().value();

// 表是否存在
db.has("posts").value();

// 设置数据
db.set("user.name", "kongzhi").write();

// get 数据, 然后添加一行数据进去，最后写入文档里面去。
db.get("posts").push({ id: 1, title: "welcome to hangzhou" }).write();

// 使用 update 更新数据
db.update("count", (n) => n + 1).write();

// 使用 mixin 混合模式来扩展我们自己的方法
db._.mixin({
  getSecondData: function (arr) {
    // 返回第二行
    return arr[1];
  },
});
// 调用 getSecondData 方法 获取到 posts 第二条数据
const xx = db.get("posts").getSecondData().value();

// 取特定字段值数组
db.get("posts").map("id").value();

// 另一种取指定数据
db.get("posts[0].id").value();
```

部分参考：
[https://www.cnblogs.com/tugenhua0707/p/11403202.html](https://www.cnblogs.com/tugenhua0707/p/11403202.html)
