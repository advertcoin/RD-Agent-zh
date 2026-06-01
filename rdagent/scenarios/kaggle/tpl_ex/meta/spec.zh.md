生成规格说明的信息

```python
def feature_eng(x: {{type of the feature}}) -> {{type of the feature}}:
    """
    
    x: np.ndarray
          {{description}}
    """
```

生成合格规格说明的标准

| 字段 | 要求 |
| -- | -- |
| description | 完整描述数据，包括维度（数量、含义、示例）|

生成的规格说明示例

```python
def feature_eng(x: {{type of the feature}}) -> {{type of the feature}}:
    """

    x: np.ndarray
        3 dimension, the meaning of the dimensions will be:
        - channel
        - high
        - width
    """
```