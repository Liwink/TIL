##### RSpec let, before

```ruby
  describe "get team a and b formations" do
    let(:match) { create :match }
    it "should get expected formations" do
      expect(match.team_a_formations).to\
        eq(match.players.includes(:match_player_relationships)\
        .where("match_player_relationships.team_name = ?", "a"))
    end
  end
```

`let()` is lazy-evaluated. This means that `let()` is not evaluated until the method that it defines is run for the first time.

```ruby
  describe "soft delete match" do
    before(:example) do
      @match = create :match
    end

    it "can be deleted" do
      expect {
        @match.destroy
      }.to change { Match.count }.from(1).to(0)
      expect(Match.find_by(id: @match.id)).to be nil
    end
```

上面例子中，就需要 `@match` 在 block 调用之前就创建，所以没有使用 `let()` 而是 `before(:example)`，它会在 describe 中每个 it 独立初始化。