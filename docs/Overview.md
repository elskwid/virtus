# Virtus Overview

Virtus provides a flexible way to add model-like behavior to Ruby objects.

## Using Virtus

Virtus features include an [attribute DSL](#), [constructor semantics](#), and [mass assignment of attributes](#). Typically, these features are mixed-in to Ruby classes or modules, and configured with the Virtus custom [builder extension interface](#).

The builder interface was built with three main use cases in mind: Models, Modules, and Value Objects. For a more in depth explanation of each see the "example" links below.

### Models

`Virtus.model` for use with a Ruby class in need of Virtus features.

```ruby
class MyModel
  # ...
  include Virtus.model
  # ...
end
```

See a full [Model Example](ModelExample.md).

### Modules

`Virtus.module` extends a module with Virtus which can be used like any other Ruby module.

```ruby
module MyModule
  # ...
  include Virtus.module
  # ...
end
```

See a full [Module Example](ModuleExample.md).

### Value Objects

`Virtus.value_object` creates a [value object](http://c2.com/cgi/wiki?ValueObject) with Virtus from a Ruby class.

```ruby
class MyValue
  # ...
  include Virtus.value_object
  # ...
end
```

See a full [Value Object Example](ValueObjectExample.md).
