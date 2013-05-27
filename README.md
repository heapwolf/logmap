# SYNOPSIS
Filter json streams/logging output using [`JSONSelect`][0] and format the output

# MOTIVATION
most tools for this have way too much ceremony and way too many features.

# INSTALL
```bash
npm install logmap -g
```

# USAGE
```bash
cat ./sample.json | logmap ".date, .loglevel, .value" -f "[%d]: (%s) %s"
```

### Input
```json
{ "date": "1369605255506", "loglevel": "info", "value": "Lorem ipsum dolor sit amet.", "title": "C++", "id": "001" }
{ "date": "1369605255507", "loglevel": "info", "value": "Mollit anim id est laborum.", "title": "Javascript", "id": "002" }
```

### Output
```
[1369605255506]: (info) Lorem ipsum dolor sit amet.
[1369605255507]: (info) Mollit anim id est laborum.
```

Because `JSONSelect` gets the data in the order that it is found, you may need 
to specify a special grammar not yet supported by JSONSelect. Below is an 
example of the grammar and data.
```bash
cat ./sample.json | logmap ".date; .loglevel; .value" -f "[%d]: (%s) %s"
```

### Input
```json
{ "title": "C++", "id": "001", "loglevel": "info", "value": "Lorem ipsum dolor sit amet.", "date": "1369605255506" }
{ "title": "Javascript", "id": "002", "loglevel": "info", "value": "Mollit anim id est laborum.", "date": "1369605255507" }
```

### Output
```
[1369605255506]: (info) Lorem ipsum dolor sit amet.
[1369605255507]: (info) Mollit anim id est laborum.
```

# OPTIONS
If you write a long query, you can save it so you don't have to write it again.

### Save
```bash
cat ./sample.json | logmap ".date, .loglevel, .value" -f "[%d]: (%s) %s" -s myQuery
```

### Load
```bash
cat ./sample.json | logmap -l myQuery
```

### Delete
```bash
logmap -d myQuery
```

### Split
If you're logs are delimited by something other than a new-line you can specify
that with the `--split` argument. By default logs are split on `/\r?\n/`.

### Format
The resulting data is applied to `console.log()` so you can use `-f` to format
the results.

[0]:http://jsonselect.org/#tryit
