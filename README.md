# Filters

`filters` allows to easily create filter objects that will allow you to filt

For showing both, the same JSON object will be used:

```python
>>> person = {
...     "name": "Alice"
...     "books": [
...         {"name": "The name of the wind"},
...         {"name": "A feast for crows"},
...         {"name": "Continuous delivery"}
...     ],
...     "age": 24,
...     "location": "Lancashire"
... }
```

## Simple Queries
We will create a filter that will return ``True`` for people living in Yorkshire.

```python
>>> from filters import Filter
>>> f = Filter(op1='.location', operator='eq', op2='Yorshire')
>>> f(person)
False
```

Complex Queries
---------------
We will create a filter that will return ``True`` when any of the book name is "A game of thrones"
or when the person is older than 18. As this specification is more complex, the
:meth:`fromConfig <filters.filters.Filter.fromConfig>` method will be used.

```python
>>> specs = {
...     "op1": {
...         "op1": ".books[].name",
...         "operator": "contains",
...         "op2": "A game of thrones"
...     },
...     "operator": "or",
...     "op2": {
...         "op1": ".age",
...         "operator": "ge",
...         "op2": 18
...     }
... }
>>> f = Filter.fromConfig(specs)
>>> f(person)
True
```

A filter can be inspected in an easier way by just prompting it:

```python
>>> f
>>> ((.books[].name - contains - A game of thrones) - or - (.age - ge - 18))
```