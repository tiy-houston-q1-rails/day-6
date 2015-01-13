# Quiz!

I've been reading [Make It Stick](http://www.amazon.com/Make-It-Stick-Successful-Learning/dp/0674729013) (A+ would recommend), and trying to do more information-retrieval practice (as a good way to make information stick). This is ungraded, but should be pretty quick.

Find the sum of these numbers. HipChat me the sum -

```
5,11,87,49,52,99
20,5,67,34,12
92,57,91,77,45,29,56,38,18,68,92,26,42,55,46
56,18,10,92,54,14,84,79
32,34,27,1.1,87,24,97
93,2,78,45,96,94,16,74,99,14,33
72,41,56,89,12,45,68,29,83,78,58,17,11,69
29,26,38,96,99,2,54
41,48,24,25,63,11,39
4,27,40,88,10,59,90
```

# Homework

I'm not your teacher, so do what you want, but I'd recommend two exercises:

* Amy is out sick this week, so Bender is taking over her deliveries. Change your solution to reflect that.
* Work in pairs to review your code. Have your partner explain to you what your code is doing without explaining it yourself first. Is there anything you can do to make it easier for them to read (e.g. naming variables, extracting methods)?

# Notes From Class

Here's the assignment, as it was when we wrapped up
```ruby
require 'csv'
require 'pry'
require 'did_you_mean'

def pilot_for_location location
  case location
  when "Earth"
    "Fry"
  when "Mars"
    "Bender" # "Amy"
  when "Uranus"
    "Bender"
  else
    "Leela"
  end
end

class Delivery
  attr_reader :destination, :shipped, :count, :profit

  def initialize destination, shipped, count, profit
    @destination = destination
    @shipped = shipped
    @count = count.to_i
    @profit = profit.to_i
  end

  def pilot
    pilot_for_location(@destination)
  end

  def bonus
    0.1 * @profit
  end
end

deliveries = []
CSV.foreach("./planet-express.txt") do |row|
  dest, item, count, profit = row # row[0], row[1], row[2], row[3]
  # { :destination => dest, :shipped => item } ~~> { destination: dest, shipped: item }
  #log = {
  #  destination: dest,
  #  shipped: item,
  #  count: count.to_i,
  #  profit: profit.to_i
  #}
  #log[:pilot] = pilot_for_location(dest)
  #logs << log
  deliveries << Delivery.new(dest, item, count, profit)
end

#total_profit = 0
#sales = CSV.foreach("./planet-express.txt") do |row|
#  profit_from_row = row[3]
#  total_profit += profit_from_row.to_i
#end
total_profit = deliveries.map { |l| l.profit }.inject(:+)

# How much money did we make this week?
puts "Total profit: #{total_profit}"

# Also, bonuses are paid to employees who pilot the Planet Express
# 
# Different employees have favorite destinations they always pilot to
# Fry - pilots to Earth (because he isn't allowed into space)
# Amy - pilots to Mars (so she can visit her family)
# Bender - pilots to Uranus (teeheee...)
# Leela - pilots everywhere else because she is the only responsible one
# 
# How many trips did each employee pilot?

# Pilot => trip counts
trip_counts = Hash.new(0) # Default value 0
deliveries.each do |delivery|
  trip_counts[ delivery.pilot ] += 1
end
puts trip_counts

# They get a bonus of 10% of the money for the shipment as the bonus
# How much of a bonus did each employee get?
bonus = Hash.new(0)
deliveries.each do |delivery|
  bonus[ delivery.pilot ] += delivery.bonus
end
bonus.each do |pilot,amount|
  puts "Pilot #{pilot} earned $#{amount} of bonus"
end


# BONUS 1
# 
# How much money did we make broken down by planet?
# ie.. how much did we make shipping to Earth? Mars? Saturn? etc.
profit_by_planet = Hash.new(0)
deliveries.each do |log|
  #if log[:destination] == "Earth"
  #  profit_by_planet["Earth"] += log[:profit]
  #elsif log[:destination] == "Mars"
  #  profit_by_planet["Mars"] += log[:profit]
  ## and so on ...
  #end
  destination = log.destination
  profit_for_trip = log.profit
  profit_by_planet[destination] += profit_for_trip
end
puts profit_by_planet

# BONUS 2 - uses classes for each shipment; but not necessary
# 
# Classes will be covered Thursday
# open("planet_express_logs.txt").each do |line|
#   string_data = line.chomp
#   * process string_data here
# end
```

# References

* [pry](http://pryrepl.org/) - see also [this talk](https://www.youtube.com/watch?v=D9j_Mf91M0I) from RubyConf
* [did_you_mean](https://github.com/yuki24/did_you_mean)

