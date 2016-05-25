# Active Record Basics

#### CRUD

##### Create

```
# new and save
user = User.create(name: "Liwink")
```

```
user = User.new
user.name = "David"
user.save
```

```
user = User.new do |u|
  u.name = "Liwink"
end
user.save
```

##### Read

```
# return a collection with all users
users = User.all

# return the first user
user = User.first

# return the first user named Liwink
liwink = User.find_by(name: "Liwink")

# find all users named Liwink and sort by created_at
users = User.where(name: "Liwink").order("created_ad DESC")
```

##### Update

```
# update and save
user = User.find_by(name: "Liwink")
user.name = "Liwink"
user.save

# use Hash
user = User.find_by(name: "Liwink")
user.update(name: "Liu")

# update several records in bulk
User.update_all "max_login_attemps = 3"
```

##### Delete

```
user = User.find_by(name: "Liwink")
user.destroy
```