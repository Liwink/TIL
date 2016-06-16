##### Rails Sentry

Rails 中集成 Sentry

config/initializers/sentry.rb

```ruby
require 'raven'
Raven.configure do |config|
  config.environments = %w[ production ]
  if Rails.env.production?
    config.dsn = Settings.storage.sentry.dsn
  end
end
```

config/settings/production.yml

```ruby
storage:
  sentry:
    dsn: http://xxx:xxx@sentry.com/2
```