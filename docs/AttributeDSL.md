# Attribute DSL

Virtus exposes a powerful attribute declaration [DSL](#) once included in an object. This DSL provides a way to declare attributes for those objects with various options to control the attribute behavior.

Attributes are declared with the following syntax: `attribute`, `name`, `type`, `options hash`.

```ruby
attribute :name, Type, options
```

_(Note: the syntax remains the same regardless of the type of object being used.)_


## Basic Example

Attributes in a class:

```ruby
class MyClass
  include Virtus.model

  attribute :title, String, :some => options
end
```

## Available Options

As mentioned before, there are options that can be specified for attributes using the DSL. The available options are:

### reader
The `attribute` declaration creates a reader which is public by default. The reader option allows an attribute reader to be set either public or private.

Possible values: `:public`, `:private`.

Default value: `:public` - the reader is publicly accessible.

Example:

```ruby
attribute :title, String, :reader => :private
```

### writer
The `attribute` declaration creates a writer which is publicy by default. The writer option allows an attribute writer to be set either public or private.

Possible values: `:public`, `:private`.

Default value: `:public` - the writer is publicly accessible.

Example:

```ruby
attribute :title, String, :writer => :private
```

### default
Supply a default value for the attribute.

Possible values: value, callable object, or a symbol representing a method name.

Default value: This option is only used if supplied.

Example:

```ruby
attribute :title, String, :default => "Some Title"
```

### lazy
Virtus sets default values during object construction. Set lazy when the default value should be set when the attribute is accessed.

Possible values: true, false.

Default value: false - defaults are set during object construction.

Example:

```ruby
attribute :title, String, :default => "Some Title", :lazy => true
```

### coerce
Virtus uses [Coercible]() to coerce input values when they are assigned by default. The coerce option indicates if Virtus will perform attribute coercion. _Note: Coercions are not performed if an attribute is mutated._

Possible values: true, false.

Default value: true - coercion is performed.

Example:

```ruby
attribute :title, String, :coerce => false
```

### strict
Virtus returns the input value even if the coercion fails. Setting strict coercion mode will force Virtus to raise an exception when the coercion fails.

Possible values: true, false.

Default value: false - Virtus returns the input value regardless of coercion success/failure.

Example:

```ruby
attribute :title, String, :strict => true
```

### required
When strict coercion is enabled (see strict option), the required option indicates if `nil` is valid coercion output.

Possible values: true, false.

Default value: true - `nil` is a valid coercion output value.

Example:

```ruby
attribute :title, String, :strict => true, :required => false
```

## More Examples

### Private Attributes

Sets the writer private for `:unique_id` and the reader private for `:token`, then attempts to access each on the instance of the class and internally.

``` ruby
class User
  include Virtus.model

  attribute :unique_id, String, :writer => :private
  attribute :token, String, :reader => :private

  def set_unique_id(id)
    self.unique_id = id
  end

  def get_token
    token
  end
end

user = User.new(:unique_id => "1234-1234", :token => "55555")

user.unique_id
# => nil

user.token
# => "55555"

user.unique_id = "1234-1234"
# => NoMethodError: private method `unique_id='

user.set_unique_id("1234-1234")

user.unique_id
# => "1234-1234"

user.token
# => NoMethodError: private method `token'

user.get_token
# => "55555"

```

### Default Values

Demonstrates various default values including: singleton values, callable object, and a method. The `views` attribute is then reset to show how the default is retained.

``` ruby
class Page
  include Virtus.model

  # lazy default title
  attribute :title, String, :default => "My Page", :lazy => true

  # default from a singleton Integer value
  attribute :views, Integer, :default => 0

  # default from a singleton Boolean value
  attribute :published, Boolean, :default => false

  # default from a callable object
  attribute :slug, String, :default => lambda { |page, attribute| page.title.downcase.gsub(' ', '-') }

  # default from a method name as symbol
  attribute :editor_title, String,  :default => :default_editor_title

  def default_editor_title
    published? ? title : "UNPUBLISHED: #{title}"
  end
end

page = Page.new(:title => 'Virtus README')

page.slug
# => 'virtus-readme'

page.views
# => 0

page.published
# => false

page.editor_title
# => "UNPUBLISHED: Virtus README"

page.views = 10

page.views
# => 10

page.reset_attribute(:views)
# => 0

page.views
# => 0
```

### Lazy Default Values

Page class with a lazy default title. If the lazy title default was used with the `slug` proc above then the title would be set during construction as the proc references `#title`. Both would need to be lazy in order to work as desired.

```ruby
class Page
  include Virtus.model

  # lazy default title
  attribute :title, String, :default => "My Page", :lazy => true

  # default from a singleton Integer value
  attribute :views, Integer, :default => 0
end

# title is not set at construction time
page = Page.new
# => #<Page:0x000... @views=0>

page.title
# => "My Page"

page.slug
# => 'virtus-readme'

page.views
# => 0
```
