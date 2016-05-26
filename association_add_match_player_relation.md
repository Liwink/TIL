## [Association] Add Match Player Relation

### 1. Model, migrate and association

##### 1.1 Generate the model

[Model Name and Polymorphism](https://ruby-china.org/topics/3733)?

```
rails generate model MatchPlayerRelationship match_id:integer player_id:integer contingent_name:string
```

##### 1.2 Add index in migration

``` ruby
  1 class CreateMatchPlayerRelationships < ActiveRecord::Migration[5.0]
  2   def change
  3     create_table :match_player_relationships do |t|
  4       t.integer :match_id
  5       t.integer :player_id
  6       t.string :contingent_name
  7
  8       t.timestamps
  9     end
 10     add_index :match_player_relationships, :match_id
 11     add_index :match_player_relationships, :player_id
 12     add_index :match_player_relationships, [:match_id, :player_id], unique: true
 13   end
 14 end
```

##### 1.3 Migrate

```
rake db:migrate
```

##### 1.4 Match has_many MatchPlayerRelationship

> In any case, Rails will not create foreign key columns for you. You need to explicitly define them as part of your migrations.

```ruby
  1 class Match < ApplicationRecord
  2   belongs_to :user
  3   belongs_to :organization
  4   belongs_to :contingent_a, class_name: 'Contingent', foreign_key: 'contingent_a_id'
  5   belongs_to :contingent_b, class_name: 'Contingent', foreign_key: 'contingent_b_id'
  6
  7   has_many :match_player_relationships,
  8     class_name: "MatchPlayerRelationship",
  9     foreign_key: "match_id",
 10     dependent: :destroy
 11 end
```

```ruby
  1 class MatchPlayerRelationship < ApplicationRecord
  2   belongs_to :match
  3   belongs_to :player
  4   validates :match_id, presence: true
  5   validates :player_id, presence: true
  6 end
```

##### 1.5 Match has_many player through match_player_relationship

```ruby
  1 class Match < ApplicationRecord
  2   belongs_to :user
  3   belongs_to :organization
  4   belongs_to :contingent_a, class_name: 'Contingent', foreign_key: 'contingent_a_id'
  5   belongs_to :contingent_b, class_name: 'Contingent', foreign_key: 'contingent_b_id'
  6
  7   has_many :match_player_relationships,
  8     class_name: "MatchPlayerRelationship",
  9     foreign_key: "match_id",
 10     dependent: :destroy
 11   has_many :players, through: :match_player_relationships, source: :player
 12 end
```

##### 1.6 Player has_many match through match_player_relationship

```ruby
  1 class Player < ApplicationRecord
  2   belongs_to :user
  3   belongs_to :organization
  4   belongs_to :contingent
  5
  6   validates :name, presence: true, allow_blank: false
  7   validates :contact, presence: true, allow_blank: false
  8
  9   has_many :match_player_relationships,
 10     foreign_key: "player_id",
 11     dependent: :destroy
 12   has_many :match, through: :match_player_relationships, source: :player
 13 end
```

### 2. Basic operation and test

##### find match players whose contingent is "a"

```ruby
match = Match.first
players = match.players.includes(:match_player_relationships).where("match_player_relationships.contingent_name = ?", "b")
```

