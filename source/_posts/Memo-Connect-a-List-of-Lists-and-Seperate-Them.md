---
title: '[ Memo ] Connect a List of Lists and Seperate Them'
date: 2019-02-20 10:14:03
tags:
	- Data Cleaning
	- Python
	- Pandas
categories:	
	- Programing
	- Python
	- Data Cleaning
---

- Result:

| {%asset_img 1.png%} | {%asset_img 2.png%} |
| ------------------- | ------------------- |
| Before              | After               |

<!--more-->

- 首先说明：由于数据量原因，下方的操作都**避免使用完整循环**结构，尽量使用pandas的操作以及列表生成式

- 首先计算每个transfers中有多少个vout

```Python
transfers['outcount'] = transfers.vout.apply(len)
```

- 根据这个outcount创建一个嵌套列表

```
counts = list(transfers.outcount)
ids = [[i]*counts[i] for i in range(len(transfers))]
```

- 然后拼接这个嵌套列表

```Python 
idss = [x for j in ids for x in j]
```

- 接下来拼接vouts

```python
vouts = [x for j in list(transfers.vout) for x in j]
vouts = pd.DataFrame(vouts)
```

- 最后，将两个拼接后的列表组合：

```python
vouts['txid'] = idss
```