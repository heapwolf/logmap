# SYNOPSIS
Filter streams of json and format the output using [`JSONSelect`][0]

# MOTIVATION
bunyan has too much ceremony and is too domain specific, jsontool isn't specific
enough for the task.

# INSTALL
```bash
npm install logmap -g
```

# USAGE
```bash
cat test/fixtures/sample.json | logmap ".date, .loglevel, .value" --format "[%d]: (%s) %s"
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
cat test/fixtures/sample.json | logmap ".date; .loglevel; .value" --format "[%d]: (%s) %s"
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

[0]:http://jsonselect.org/#tryit
