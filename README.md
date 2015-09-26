# SYNOPSIS
Filter new-line-deleimted json streams using simple object selectors.

# MOTIVATION
your tool has too many features.

# INSTALL
```bash
npm install logmap -g
```

# USAGE
### INPUT
```json
{ "date": "2015-06-25T01:14:21.196+0600", "level": "info", "value": { "msg": "Lorem ipsum dolor sit amet.", "title": "C++", "id": "001" } }
{ "date": "2015-04-25T01:32:21.196+0600", "level": "info", "value": { "msg": "Mollit anim id est laborum.", "title": "Javascript", "id": "002" } }
```

### COMMAND
```bash
cat ./sample.json | logmap date level value.msg -f "[%date(DD HH:mm)]: (%s) %s"
```

### OUTPUT
```
[14 21:32]: (info) Lorem ipsum dolor sit amet.
[24 21:32]: (info) Mollit anim id est laborum.
```

### FUN STUFF
- Neglecting the `-f` option will just output an array of the selected values.
- You can use any of the object selectors found [`here`](https://github.com/mariocasciaro/object-path#usage)
- You can do `%color(inverse.red)`, stuff in the parens invokes [`chalk`](https://github.com/chalk/chalk)
- You can do `%date(MM YY d:mm)`, stuff in the parens invokes [`moment`](https://github.com/moment/moment)
- You can do regluar things like `%s`, `%s` and `%j`.

# OPTIONS
If you write a long query, you can save it so you don't have to write it again.

### SAVE
```bash
cat ./sample.json | logmap ".date, .loglevel, .value" -f "[%d]: (%s) %s" -s myQuery
```

### LOAD
```bash
cat ./sample.json | logmap -l myQuery
```

### DELETE
```bash
logmap -d myQuery
```

[0]:http://jsonselect.org/#tryit

