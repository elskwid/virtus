# Using Virtus.model

Include `Virtus.model` in a class to add Virtus features. The including class can be used like any other Ruby class.

```ruby
class User
  include Virtus.model

  attribute :name, String
  attribute :age, Integer
  attribute :birthday, DateTime
end

user = User.new(:name => 'Piotr', :age => 29)

user.attributes
# => { :name => "Piotr", :age => 29 }

user.name
# => "Piotr"
```
