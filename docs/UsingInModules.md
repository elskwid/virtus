# Using Virtus in Modules

Include `Virtus.module` to create a module extened with Virtus. The extended
module provides a way to define attributes for later inclusion in to other
classes:

```ruby
# module with name attribute
module Name
  include Virtus.module

  attribute :name, String
end

# module with age attribute and no coercion
module Age
  include Virtus.module(:coerce => false)

  attribute :age, Integer
end

class User
  include Name, Age
end

user = User.new(:name => 'John', :age => 30)
```

[Contents](_contents.md)
