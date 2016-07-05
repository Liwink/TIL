# 国际化

### 基础功能

自定义的翻译词条存放在 `config/locales` 下，使用 `YAML` 格式。Rails 会自动把 `config/locales` 文件夹中所有 `.rb` 和 `.yml` 文件加入**译文加载路径**。

```yaml
"zh-CN":
  organizations:
    target_contingent_is_invalid: '班级 参数错误'
```

> 就算你的网站不需要支持多国语言，这个功能对团队协助开发仍然非常有帮助，因为开发时不一定会先确定文案，用 i18n 来处理话，最后只需要让 PM 统一修改翻译文案即可。

在 Rails 中可以 `I18n.t` 方法来做翻译词条的替换，翻译关键字可以用字符串或 Symbol：

```ruby
I18n.t("organizations.target_contingent_is_invalid")
I18n.t :name_is_invalid, :scope=>:organizations
```



### Globalize

#### 安装

[Globalize](https://github.com/jdbwin/globalize) 对 Rails 5 支持不够好

> When using bundler put this in your Gemfile:
>
> `gem 'globalize', '~> 5.0.0'`
>
> To use globalize with Rails 5 add this in your Gemfile
>
> `gem 'activemodel-serializers-xml'`

但官方版在 Rails Console 时会报错，相关 issue 有讨论 [Undefined method 'type_cast_from_database' ](https://github.com/globalize/globalize/issues/502)，原因是调用了 Rails5 中遗弃的方法。所以最后选择了 issue 中修复后的个人 repo。

#### 使用

在 Model 中设置需要翻译的字段

```ruby
class Contingent < ApplicationRecord
  translates :name, :string
end
```

编写迁移脚本

```ruby
class TranslateContingents < ActiveRecord::Migration[5.0]
  def self.up
    Contingent.create_translation_table!({
      :name => :string
    }, {
      :migrate_data => true
    })
  end

  def self.down
    Contingent.drop_translation_table! :migrate_date => true
  end
end
```

设置不同 `I18n.locale` 下的字段值

```ruby
I18n.locale # 'zh-CN'
c = Contingent.find_by("name = 'phantom'")
c.name # 'phantom'
c.update_attribute(:name, "未分配班级") # c.attributes = { name: 'phantom', locale: :en }
c.name # "未分配班级"
I18n.locale = :en
c.name # 'phantom'
```

#### 原理

`git diff db/schema.rb`

```ruby
create_table "contingent_translations", force: :cascade do |t|
  t.integer  "contingent_id", null: false
  t.string   "locale",        null: false
  t.datetime "created_at",    null: false
  t.datetime "updated_at",    null: false
  t.string   "name"
  t.index ["contingent_id"], name: "index_contingent_translations_on_contingent_id", using: :btree
  t.index ["locale"], name: "index_contingent_translations_on_locale", using: :btree
end
```

当为 contingent 创建翻译字段时，会创建一个新的 table 存储翻译信息，而并不直接修改原 contingents table。



#### 坑

https://github.com/spree-contrib/spree_i18n/pull/513

https://github.com/globalize/globalize/issues/263





