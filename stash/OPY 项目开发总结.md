## OPY 项目开发总结

##### 技术

Best Practice

- MCV，Controller 和 Model 独立，重 Model 轻 Controller，Serializer 配置返回结构
- Ruby 迷幻的语法
- 丰富友好的组件：FactoryGirl 组件，和 Serializer 很像，在 OpenPlay 项目中我也会用，但没有 Gem 支持，很难做到独立配置，便捷使用；RSpec，有
- 回调 Callback



##### 更好的代码

- Best Practice

  - Rails 文件目录更加规范和强制，明确什么样的信息应该放在哪里；
  - 每个功能都有优秀、突出的库，例如：FactoryGirl、cancancan、Serializer、api-pagination；
  - Gem 比 PIP 更优秀；
  - 强大的 ORM：框架和数据直接解决了 OpenPlay 中头疼的「原子性」问题，Model 中自带方法非常实用丰富，好用的 scope；
  - Ruby 的元编程，更加灵活开发的语法，支持 Best Practice 的实现；

- Code Review

  - 实现功能、完整、易读；

- Python 和 Tornado

  - > Explicit is better than implicit.

    Rails 中约定优于配置，减少了代码量，但并不如 Python 中清晰。例如：Rails 中的包都是一并放在项目初始化中引入，每个文件不需再引入，Python 中则是只有当指定文件需要时才会引入；Rails 中 Model、Migration、Schema 和数据库之间有较复杂约定关系，例如运行一个迁移脚步后，数据库会完成迁移字段的改变，Schema 中会更新，Model 可以自动引用新引进的字段，他们之间功能独立，但通过约定管理起来；

  - Tornado 和相关的库更新太不活跃，大多一年以上没人维护，较 Rails 相差太远。

##### 更完整的项目经验

- 需求分析：由于项目类似，前期自己并没有进行完整的需求分析，还是欠缺；
- 数据库设计：同上；
- 接口设计：
- 文档规范：
- 时间安排：
- 迭代开发：
- 测试提测：
- 部署上线：

##### 更负责任的团队合作

- GitHub issues：非常棒的引入，对自己工作安排、团队合作、问题记录都特别有益；
- 文档：这块自己太不重视，由于合作人较少，自己一直习惯面对面口头交流，但这对项目长期维护并不有利，另一方面，如何高效生成文档也很重要，DRY；
- 进度：需要对进度负责，可以前期调整进度，但决不可无视进度；



