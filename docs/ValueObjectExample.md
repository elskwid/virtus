### Value Objects

``` ruby
class GeoLocation
  include Virtus.value_object

  values do
    attribute :latitude,  Float
    attribute :longitude, Float
  end
end

class Venue
  include Virtus.value_object

  values do
    attribute :name,     String
    attribute :location, GeoLocation
  end
end

venue = Venue.new(
  :name     => 'Pub',
  :location => { :latitude => 37.160317, :longitude => -98.437500 })

venue.location.latitude # => 37.160317
venue.location.longitude # => -98.4375

# Supports object's equality

venue_other = Venue.new(
  :name     => 'Other Pub',
  :location => { :latitude => 37.160317, :longitude => -98.437500 })

venue.location === venue_other.location # => true
```
