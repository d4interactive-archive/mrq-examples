# MRQ Task Queue Examples

## Tasks

Following are the tasks classes reside inside tasks.py:

- test_set
- test_sorted_set
- test_raw

With this guideline, you can easily understand the behavior of each task queue.

An example test case file is test.py, you can run that file and workers separately to monitor 
closely how this works.

## Workers

To understand a correct behavior for the set and sorted set task, you need to 
run the following workers:

#### Set Worker

```
mrq-worker test_set
```

When items are pushed to the queue with a following payload:

```
urls = [
    ['https://contentstudio.io'],
    ['https://d4interactive.io'],
    ['https://techcrunch.com'],
    ['https://mashable.com'],
    ['https://mashable.com']
]

urls_key_value = [
    {
        'url': 'https://techcrunch.com'
    },
    {
        'url': 'https://mashable.com'
    },
    {
        'url': 'https://techcrunch.com'
    },
]
```

Expected output should be:

```
Set Payload {'set': b"['https://mashable.com']"}
Set Payload {'set': b"['https://contentstudio.io']"}
Set Payload {'set': b"['https://techcrunch.com']"}
Set Payload {'set': b"['https://d4interactive.io']"}
Set Payload {'set': b"{'url': 'https://mashable.com'}"}
Set Payload {'set': b"{'url': 'https://techcrunch.com'}"}
```

**NOTE:** Duplicate keys will be not be added.


#### Sorted Set Worker
```
mrq-worker test_sorted_set
```

When items are pushed to the queue in sorted set case with the below URLs:

```
urls = [
    ['https://contentstudio.io', 10],
    ['https://d4interactive.io', 5],
    ['https://techcrunch.com', 0],
    ['https://techcrunch.com', -10],
    ['https://mashable.com', -25]
]
```
 - First index is a payload, second index is a priority that needs to be passed for the mrq.
 - Score with the negative value will be processed at earliest.

Expected output:

```
{'sorted_set': b'https://mashable.com'}
{'sorted_set': b'https://techcrunch.com'}
{'sorted_set': b'https://d4interactive.io'}
{'sorted_set': b'https://contentstudio.io'}
```

**NOTE:** Duplicate URLs/payload will be ignored in sorted sets as it only allows unqiue keys.

 
#### Raw Worker

```
mrq-worker test_raw
```
When items are pushed to the queue with a following payload:

```
urls = [
    ['https://contentstudio.io'],
    ['https://d4interactive.io'],
    ['https://techcrunch.com'],
    ['https://mashable.com'],
    ['https://mashable.com']
]

urls_key_value = [
    {
        'url': 'https://techcrunch.com'
    },
    {
        'url': 'https://mashable.com'
    },
    {
        'url': 'https://techcrunch.com'
    },
]
```

In case of Raw, duplicate values will not be ignored as shown below:

```
Raw Payload {'sorted_set': b"{'url': 'https://techcrunch.com'}"}
Raw Payload {'sorted_set': b"{'url': 'https://mashable.com'}"}
Raw Payload {'sorted_set': b"{'url': 'https://techcrunch.com'}"}
Raw Payload {'sorted_set': b"['https://contentstudio.io']"}
Raw Payload {'sorted_set': b"['https://d4interactive.io']"}
Raw Payload {'sorted_set': b"['https://techcrunch.com']"}
Raw Payload {'sorted_set': b"['https://mashable.com']"}
Raw Payload {'sorted_set': b"['https://mashable.com']"}
```


## TODO

- [ ] Timed set example.
- [ ] Use cases solved through MRQ and optimized settings with MRQ.

### Resources:
 
 - https://redis.io/topics/data-types (For redis data types like List, Sorted Sets, Sets, HyperLogLogs)