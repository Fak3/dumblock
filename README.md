This package provides decorators that will try to acquire redis lock before calling decorated
function.

## Installation

```bash
pip install dumblock
```

## Usage

Set `DUMBLOCK_REDIS_URL` in your django settings:

```python
DUMBLOCK_REDIS_URL = "redis://localhost:6379"
```

### lock_or_exit

Decorator. Before decorated function starts, try to acquire redis lock
with specified key. If lock is acquired successfully, proceed executing
the function. Otherwise, return immediately.
The `key` argument can contain templated string, wich will be rendered
with args and kwargs, passed to the function.

Example:

```python
@lock_or_exit('lock_work_{}')
def workwork(x):
    pass

workwork(3)  # Will try to acquire redis lock 'lock_work_3'
```

### lock_wait

Decorator. Before decorated function starts, try to acquire redis lock
with specified key, waiting for `waittime` seconds if needed. If lock is
acquired successfully, proceed executing the function. Otherwise, raise
`dumblock.TimeoutError`.
The `key` argument can contain templated string, wich will be rendered
with args and kwargs, passed to the function.

Example:

```python
@lock_wait('lock_work_{}', waittime=4)
def workwork(x):
    pass

workwork(3)  # Will try to acquire redis lock 'lock_work_3' for 4 seconds
```
