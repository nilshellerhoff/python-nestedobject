# python-nestedobject
Extend Python's Dict and List classes with convenient filter and map methods.

Easily filter and map big nested objects (dicts and lists). Works similar to JavaScript's Array.filter() and Array.map() methods.

Consider this example:

```python
cars = [
    { "manufacturer": "Audi", "models": [
        { "name": "A1", "year": 2010 },
        { "name": "A4", "year": 2013 }
    ]},
    { "manufacturer": "BMW", "models": [
        { "name": "X1", "year": 2011 },
        { "name": "X5", "year": 2014 }
    ]},
    { "manufacturer": "Mercedes", "models": [
        { "name": "C-class", "year": 2012 },
        { "name": "E-class", "year": 2015 }
    ]}
]
```

Assume we want to get the year of the X5. In standard Python, we would have to write something like this:

```python
[[car2['year'] for car2 in car['models'] if car2['name'] == 'X5'][0] for car in cars if car['manufacturer'] == 'BMW'][0]
```

With this class we can do the following:
```python
cars2 = NestedObject(cars)
cars2.filter(lambda x: x['manufacturer'] == 'BMW')[0]['models'].filter(lambda x: x['name'] == 'X5')[0]['year']
```

or using the (unsafe) string index filter:
```python
cars2['_["manufacturer"] == "BMW"'][0]['models']['_["name"] == "X5"'][0]['year']
```

## Installing

Copy `nestedobject.py` in your folder.

## Usage

```python
from nestedobject import NestedObject
cars2 = NestedObject(cars)
```

**Filtering**

All cars by BMW:
```python
cars2.filter(lambda _: _["manufacturer"] == "BMW")
```

There is a second syntax for list filtering using string expressions evaluated using `eval()`. **This syntax is unsafe, don't ever use it with unsanitized inputs!** 

```python
cars2['_["manufacturer"] == "BMW"']
```

In this syntax, you can use any kind of expression, where `_` will be translated to the current element.