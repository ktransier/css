#### Kill server

```
lsof -wni tcp:3000
kill -9 PID
```

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
