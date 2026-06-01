## 特征工程

- 实现一个特征工程函数，具有以下签名：

```python
def feat_eng(X: np.ndarray, y: np.ndarray | None = None, X_fit: np.ndarray | None = None, y_fit: np.ndarray | None = None, param: object | None = None) -> tuple[np.ndarray, np.ndarray | None, object]:
    """
    对输入数据执行特征工程。

    参数：
    - X: np.ndarray
        要转换的输入数据。
    - y: np.ndarray | None
        目标数据。
    - X_fit: np.ndarray | None
        用于拟合转换参数的数据。
    - y_fit: np.ndarray | None
        用于拟合的目标数据。
    - param: object | None
        预先拟合的转换参数。

    返回：
    - transformed_data: np.ndarray
        转换后的数据。
    - transformed_target: np.ndarray | None
        转换后的目标数据。
    - fitted_param: object
        拟合后的参数。
    """
```

- 确保特征工程过程一致，并且可以应用于训练和测试数据。