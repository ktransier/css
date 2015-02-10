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
