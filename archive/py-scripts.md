# Python scripts

## create virtualenv

```bash
mkvirtualenv my_env
```

## hidden features

- **Extended Unpacking**:

```py
# python 3.x
x, *_, y = (1, 2, 3, 4, 5)
# x = 1, y = 5
```

## hidden stdlib methods

- `textwrap`:

```py
textwrap.shorten(article, 30, placeholder='...')
# 'Lorem Ipsum is simply dummy...'

print(textwrap.fill(text, width=20))
# Lorem Ipsum is
# simply dummy text of
# the printing and
# typesetting

textwrap.indent(quote, prefix='> ')
# '> Lorem Ipsum is simply dummy text'
```

- `string.capwords`:

```py
string.capwords('Lorem Ipsum is simply dummy text')
# 'Lorem Ipsum Is Simply Dummy Text'
```

- `str.rjust`:

```py
'2345'.rjust(16, '#')
'############2345'
```

- `fnmatch`:

```py
fruits = ['apple', 'banana', 'orange', 'lemon']
fnmatch.filter(fruits, '*o*')
# ['orange', 'lemon']
```

```py
fnmatch.fnmatch('Duck', 'D??k')
# True
```

```py
fnmatch.translate('*Drapegnik*')
# '(?s:.*Френк.*)\\Z'
```

- `difflib`:

```py
difflib.SequenceMatcher(None, 'foo', 'bar').ratio() # 0.0
difflib.SequenceMatcher(None, 'foo', 'foo').ratio() # 1.0

a = 'Самый тяжёлый китаец похудел за полгода на 142 кг'
b = 'Шок! Житель китая скинул 142 кг! Всего за полгода!'

difflib.SequenceMatcher(None, a.lower(), b.lower()).ratio()
# 0.42424242424242425
```

- `pprint` `depth`:

```py
import json, urllib, pprint

repos = json.loads(urllib.request.urlopen('https://api.github.com/search/repositories?q=react').read())

pprint.pprint(repos, depth=1)
# {'incomplete_results': False, 'items': [...], 'total_count': 761826}
```

- `shlex`:

> split text into words with save phrases in quotes

```py
import shlex
from collections import Counter

words = shlex.split(text)
Counter(words).most_common(3)
# [('Lorem Ipsum', 4), ('dummy', 2), ('text', 2)]
```

- `collections.ChainMap`:

```py
from collections import ChainMap

DEFAULTS = {
  'host': 'localhost',
  'port': 3000,
}
custom = { 'port': 3001 }

settings = ChainMap(custom, DEFAULTS)

settings['port']
# 3001

settings['host']
# 'localhost'
```

- `collections.defaultdict`:

```py
from collections import defaultdict
stats = defaultdict(lambda: 0)

stats['foo'] += 1
dict(stats)
# {'foo': 1}
```

- `collections.Counter`

```py
stats = Counter(req.method for req in requests)
dict(stats)
# {'PUT': 1, 'GET': 3, 'POST': 2}
```

```py
monday = {'PUT': 1, 'GET': 3, 'POST': 2}
tuesday = {'PUT': 5, 'GET': 10, 'POST': 5}
wednesday = {'GET': 1, 'POST': 6, 'DELETE': 2}

monday = Counter(monday)
tuesday = Counter(tuesday)
wednesday = Counter(wednesday)

stats = monday + tuesday + wednesday
# Counter({'GET': 14, 'POST': 13, 'PUT': 6, 'DELETE': 2})

stats.most_common(1)
# [('GET', 14)]

(tuesday | wednesday).keys()
# dict_keys(['PUT', 'GET', 'POST', 'DELETE'])

(tuesday & wednesday).keys()
# dict_keys(['GET', 'POST'])

(stats - monday)['GET']
# 11
```

```py
sum(map(Counter, [monday, tuesday, wednesday]), Counter())
# Counter({'GET': 14, 'POST': 13, 'PUT': 6, 'DELETE': 2})
```

- `collections.deque`:

> tail `n`

```py
buffer = deque(maxlen=3)

buffer.append(1)
buffer.append(2)
buffer.append(3)
buffer.append(4)
buffer.append(5)

list(buffer)
# [3, 4, 5]
```

- `collections.namedtuple`:

```py
User = namedtuple('User', 'id name age')

bob = User(id=1, name='Bob', age=42)
bob._replace(age=43)
dict(bob._asdict())
# {'id': 1, 'name': 'Bob', 'age': 42}
```

- `float.as_integer_ratio`:

```py
(0.75).as_integer_ratio() # ❌
# (3, 4)

(0.2).as_integer_ratio() # ❌
# (3602879701896397, 18014398509481984)

Decimal('0.2').as_integer_ratio() # ✅
# (1, 5)
```

- `namedtuple` `defaults`:

> Python 3.7+

```py
User = namedtuple('User', 'id name age role', defaults=('Anonymous', None, 'junior'))

User(1)
# User(id=1, name='Anonymous', age=None, role='junior')
User(id=2, role='senior', age=27)
# User(id=2, name='Anonymous', age=27, role='senior')
```

- `heapq.merge`

> merge sorted lists

```py
england = [
  (27, 'Harry Kane'),
  (25, 'Kun Aguero'),
  (24, 'Mohamed Salah')
]

spain = [
  (30, 'Lionel Messi'),
  (25, 'Gareth Bale'),
  (23, 'Luis Suarez')
]

germany = [
  (26, 'Robert Lewandowski'),
  (24, 'Marco Reus'),
  (21, 'Mario Götze')
]

all = heapq.merge(england, spain, germany, reverse=True)

next(all)
# (30, 'Lionel Messi')
next(all)
# (27, 'Harry Kane')
```

- `heapq.nlargest`:

> top `k` with unsorted list

```py
max(results) # k = 1

heapq.nlargest(k, contenders) # with "small" k

sorted(contenders)[-k:] # with "big" k
```

- prioritet queue `heapq`:

```py
requests = []
heapq.heappush(requests, (-1, time.time_ns(), 'Bob'))
heapq.heappush(requests, (-1, time.time_ns(), 'Alice'))
heapq.heappush(requests, (-10, time.time_ns(), 'Flesh'))

heapq.heappop(requests)
# (-10, 1550591864519931000, 'Flesh')
heapq.heappop(requests)
# (-1, 1550591864044049000, 'Bob')
```
