### Using Virtus.value_object

A Value Object can be created using `Virtus.value_object`. In Virtus, a value object is one that is immutable and compared based on state, rather than identity which is the usual case in Ruby.

``` ruby
class GeoLocation
  include Virtus.value_object

  values do
    attribute :latitude,  Float
    attribute :longitude, Float
  end
end

location = GeoLocation.new(
  :latitude => 37.160317,
  :longitude => -98.437500
)

location.latitude
# => 37.160317

location.longitude
# => -98.4375
```

Value objects are "immutable":

```ruby
location.latitude = 38
# => NoMethodError: private method `latitude=' called for #<GeoLocation latitude=37.160317 longitude=-98.4375>
```

Value objects support object equality based on state:

```ruby
other_location = GeoLocation.new(
  :latitude => 37.160317,
  :longitude => -98.437500
)

location === other_location
# => true
```

Cloning a value object retains the value (actually returning the same value object instance):

```ruby
cloned_location = location.clone
location.eql? cloned_location
# => true

location.object_id == cloned_location.object_id
# => true
```
