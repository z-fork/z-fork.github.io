---
title: snowflake
layout: post
tags:
- python
- 分布式
---

### 分布式自增 id 算法

有关 snowflake 的由来网上已经有相当多的叙述, 若未通说过自行 google. 

#### snowflake(64bits)

| timestamp | datacenter | worker | sequence |
| :-------: | :--------: | :----: | :------: |
| 41bits    | 5bits      | 5bits  | 12bits   |

[snowflake](https://github.com/falcondai/python-snowflake/blob/master/snowflake.py)

~~~ python
import datetime

# twitter's snowflake parameters
twepoch = 1288834974657L
datacenter = 5L
worker = 5L
sequence = 12L
max_datacenter_id = 1 << datacenter
max_worker_id = 1 << worker
max_sequence_id = 1 << sequence
max_timestamp = 1 << (64L - datacenter - worker - sequence)


def make_snowflake(timestamp_ms, datacenter_id, worker_id, sequence_id, twepoch=twepoch):
    """generate a twitter-snowflake id
    :param: timestamp_ms time since UNIX epoch in milliseconds"""

    sid = ((int(timestamp_ms) - twepoch) % max_timestamp) << datacenter << worker << sequence
    sid += (datacenter_id % max_datacenter_id) << worker << sequence
    sid += (worker_id % max_worker_id) << sequence
    sid += sequence_id % max_sequence_id

    return sid


def melt(snowflake_id, twepoch=twepoch):
    """inversely transform a snowflake id back to its parts."""
    sequence_id = snowflake_id & (max_sequence_id - 1)
    worker_id = (snowflake_id >> sequence) & (max_worker_id - 1)
    datacenter_id = (snowflake_id >> sequence >> worker) & (max_datacenter_id - 1)
    timestamp_ms = snowflake_id >> sequence >> worker >> datacenter
    timestamp_ms += twepoch

    return timestamp_ms, int(datacenter_id), int(worker_id), int(sequence_id)


def local_datetime(timestamp_ms):
    """convert millisecond timestamp to local datetime object."""
    return datetime.datetime.fromtimestamp(timestamp_ms / 1000.)


if __name__ == '__main__':
    import time
    t0 = int(time.time() * 1000)
    print local_datetime(t0)
    assert(melt(make_snowflake(t0, 0, 0, 0))[0] == t0)
~~~

#### TODO

* redis incrby
* 进程 or 线程安全? 
 * 如果架构是多进程, worker - pid & sequence_id 全局变量?
 * 线程呢? (python 局限于 PIL, 多进程端口映射... so 不用考虑?) 独立服务?
