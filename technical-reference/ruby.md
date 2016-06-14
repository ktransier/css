#### [Continue a loop after catching an exception](http://stackoverflow.com/a/4154966/4233556)
```ruby
for i in 1..5
  begin
    do_something_that_might_raise_exceptions(i)
  rescue
    puts $!, $@
    next    # do_something_* again, with the next i
   end
end
```

#### [!](http://stackoverflow.com/a/612653/4233556)

#### Iterator method
```ruby
3.times { print "Ruby!" }
1.upto(9) { |x| print x }
```

#### Basic Hash

h = {one: 1, two: 2}
h[:one]

#### Iteration through string

```ruby
s.each_char {|x| print "#{x} " }
```

#### Alternate array literal syntax for strings

```ruby
words = %w[this is a test]
```

#### Convert range to array
```ruby
a = ('a'..'e').to_a
```

#### Obtain the first three elements of the array
```ruby
a[0..2]
```

#### Substitute subarray
```ruby
a[0..2] = a[1..3]
```

#### Subtract elements from an array
```ruby
['a', 'b', 'c', 'b', 'a'] - ['b', 'c', 'd']
```

#### Include for ranges and arrays

```ruby
cold_war = 1945..1989
cold_war.includes? 1962
```

#### Determine class
```ruby
b = "Batman"
b.class
b.class.superclass
b.instance_of? String
```
#### Append to array
```ruby
a = (1..5).to_a
a.push(6)
a << 6
```

#### 4 types of variables and constant
```ruby
BATMAN = "Batman" #constant
$batman = "Batman" #global variable
@@batman = "Batman" #class variable
@batman = "Batman" #instance variable
_batman = "Batman" #local variable for within methods
```

#### [::](http://stackoverflow.com/a/3009565/4233556)

#### [Virtual Attribute](http://stackoverflow.com/a/5399010/4233556)

#### Abbreviated assignment pseudooperators

```ruby
x += y  # x = x + y
x -= y  # x = x - y
x *= y  # x = x * y
x /= y  # x = x / y
x %= y  # x = x % y
x **= y # x = x ** y 
x &&= y # x = x && y 
x ||= y # x = x || y
x &= y  # x = x & y
x |= y  # x = x | y
x ^= y  # x = x ^ y
x <<= y # x = x << y
x >>= y # x = x >> y 
```

#### Ternary operator
```ruby
val = a ? b : c
```

#### Loops

```ruby
for x in array do
  code
end

hash = {:a=>1, :b=>2, :c=>3}
for key,value in hash
  puts "#{key} => #{value}" 
end

while x == true do
 code
end

until x == true do
  code
end
```

#### Collect (map), select, reject, and inject
```ruby
squares = [1,2,3].collect { |x| x * x } # => [1, 4, 9]
evens = (1..10).select { |x| x % 2 == 0 } # => [2,4,6,8,10]
odds = (1..10).reject { |x| x % 2 == 0 } # => [1,3,5,7,9] 
sum = [2, 5, 3, 4].inject {|sum, x| sum + x } # => 14
```

#### Yield

```ruby
# Generate n points evenly spaced around the circumference of a
# circle of radius r centered at (0,0). Yield the x and y coordinates # of each point to the associated block.
def circle(r,n)
  n.times do |i| 
    # Notice that this method is implemented with a block angle = Math::PI * 2 * i / n
    yield r*Math.cos(angle), r*Math.sin(angle)
  end 
end
# This invocation of the iterator prints:
# (1.00, 0.00) (0.00, 1.00) (-1.00, 0.00) (-0.00, -1.00) 
circle(1,4) {|x,y| printf "(%.2f, %.2f) ", x, y }
```

#### Each with pair

```ruby
{:one=>1}.each_pair {|key,value| ... }
```
