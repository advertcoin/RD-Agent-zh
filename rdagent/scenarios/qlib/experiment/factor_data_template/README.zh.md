# 如何读取文件

例如，如果你想读取 `filename.h5`：

```python
import pandas as pd
df = pd.read_hdf("filename.h5", key="data")
```

注意：**所有 hdf5 文件的 key 始终为 "data"**。

# 数据简要说明

| 文件名 | 描述 |
|--------|------|
| "daily_pv.h5" | 调整后的每日价格和成交量数据 |

# 不同数据的基本知识

## 每日价格和成交量数据

$open: 当日股票开盘价
$close: 当日股票收盘价
$high: 当日股票最高价
$low: 当日股票最低价
$volume: 当日股票成交量
$factor: 当日股票因子值