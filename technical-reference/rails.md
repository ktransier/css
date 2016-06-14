#### Killing server
```
lsof -wni tcp:3000
kill -9 PID
```

#### Rabl
If using Rabl in combination with Haml, must rename application.haml to application.html.haml [+](http://stackoverflow.com/a/10443301/4233556)

Rabl files in rails must include format, aka users.json.rabl, not users.rabl

#### Angular + Rails
Angular + Rails http://robots.thoughtbot.com/avoid-angularjs-dependency-annotation-with-rails
