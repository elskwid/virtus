# Using Virtus.module

Include `Virtus.module` to create a module extened with Virtus. The extended module provides a way to define attributes for later inclusion in to other classes:

```ruby
# module with name attribute
module Name
  include Virtus.module

  attribute :name, String
end

# module with age attribute
module Age
  include Virtus.module

  attribute :age, Integer
end

class User
  include Name, Age
end

user = User.new(:name => 'John', :age => 30)

user.attributes
# => { :name => "John", :age => 30 }

user.name
# => "John"
```
