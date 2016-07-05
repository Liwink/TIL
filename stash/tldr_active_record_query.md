## tldr Active Record Query

##### find
Model.find(primary_key)，获取指定主键对应的对象

```ruby
client = Client.find(10)
```

##### take
Model.take，获取一个记录

```ruby
client = Client.take
```

Model.take(n)，返回指定数量的记录

```ruby
clients = Client.first(2)
# => [
  #<Client id: 1, first_name: "Lifo">,
  #<Client id: 12, first_name: "Fifo">
#]
```

##### first
Model.first，获取按主键排序的第一个记录

```ruby
client = Client.first
```

Model.first(n)，返回指定数量的记录

```ruby
clients = Client.first(3)
# => [
  #<Client id: 1, first_name: "Lifo">,
  #<Client id: 2, first_name: "Fifo">,
  #<Client id: 3, first_name: "Filo">
#]
```

##### last
Model.last，获取按主键排序的最后一个记录

```ruby
client = Client.last
```

Client.last(n)，返回指定数量的记录

```ruby
clients = Client.first(3)
# => [
  #<Client id: 111, first_name: "Lifo">,
  #<Client id: 112, first_name: "Fifo">,
  #<Client id: 113, first_name: "Filo">
#]
```

##### find_by
Model.find_by，获取满足条件的一个记录

```ruby
Client.find_by first_name: 'Lifo'
```

##### find_each
```ruby
User.find_each do |user|
  NewsMailer.weekly(user).deliver_now
end
```

##### find_in_batches
```ruby
Invoice.find_in_batches do |invoices|
  export.add_invoices(invoices)
end
```
