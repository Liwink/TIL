##### Rails Console 中使用 Pry

> New to Rails 4 is the ability to supply a block to console, a method that is only evaluated when the Rails environment is loaded through the console. This allows you to set console-specific configurations, such as using Pry over IRB.

/config/environments/development.rb
```ruby
console do# this block is called only when running console, # so we can safely require pry hererequire "pry"config.console = Pryend
```
