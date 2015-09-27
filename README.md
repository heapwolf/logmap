# SYNOPSIS
Filter & format new-line-deleimted json streams using simple object-path selectors.

# MOTIVATION
This is not a json transform tool. It's just a simple filter & format tool.

# INSTALL
```bash
npm install logmap -g
```

# USAGE
Given some program is outputting newline-delimted-json...
```json
{ "date": "2015-06-25T01:14:21.196+0600", "level": "info", "value": { "msg": "Lorem ipsum dolor sit amet.", "sources": ["C++", "JS"] } }
{ "date": "2015-04-25T01:32:21.196+0600", "level": "info", "value": { "msg": "Mollit anim id est laborum.", "srouces": ["C++", "JS"] } }
```

Pipe it to the following the reduce the output using simple object-path selectors...

```bash
cat ./sample.json | logmap date level value.msg
["2015-06-25T01:14:21.196+0600", "info", "Lorem ipsum dolor sit amet."]
["2015-04-25T01:32:21.196+0600", "info", "Mollit anim id est laborum."]
```

## ARRAYS & ACCESSING VALUES
You can also access items in an array like this `foo.bar.1.thing`. In fact, you can use any 
of the object selectors found [`here`](https://github.com/mariocasciaro/object-path#usage).

## FORMATTING OUTPUT
sprintf-ish things like `%s`, `%80s`, `%d` and `%j`.
```bash
cat ./sample.json | logmap date level value.msg -f "[%moment(DD HH:mm)]: (%s) %s"
[14 21:32]: (info) Lorem ipsum dolor sit amet.
[24 21:32]: (info) Mollit anim id est laborum.
```

### COLOR
`%chalk(inverse.red)`, invokes [`chalk`](https://github.com/chalk/chalk). there are a few
shortcuts like `%red`, `%blue`, etc. use `%chalk(...)` for more complicated things.

### DATES
Anything inside the `%moment(...)` like `%moment(MM YY d:mm)` just invokes [`moment`](https://github.com/moment/moment)

### EXPRESSIONS
Sometimes you dont want to putput a record based on some simple boolean logic, for that
you can use quoted expressions with `>`, `<`, `>=`, `<=`, `==`, `!=`.

```
cat ./sample.json | log.name log.message "log.priority > 2"
cat ./books.json | log.name "title == \"Programming\""
```

# OPTIONS
If you write a long query, you can save it so you don't have to write it again.

### SAVE
```bash
cat ./sample.json | logmap user.name.last, user.name.first, user.age -f "%s, %s (%s)" -s myQuery
```

### LOAD
```bash
cat ./sample.json | logmap -l myQuery
```

### DELETE
```bash
logmap -d myQuery
```

