## 集成和决策

- 实现一个集成和决策函数，具有以下签名：

```python
def ensemble_workflow(test_pred_l: list[np.ndarray], val_pred_l: list[np.ndarray], val_label: np.ndarray) -> np.ndarray:
    """
    处理以下内容：
    1) 使用简单平均集成预测。
    2) 集成后做出最终决策（将预测转换为最终形式）。

    参数
    ----------
    test_pred_l : list[np.ndarray]
        测试数据的预测列表。
    val_pred_l : list[np.ndarray]
        验证数据的预测列表。
    val_label : np.ndarray
        验证数据的真实标签。

    返回
    -------
    np.ndarray
        测试数据的预测结果。
    """
```

- 该函数应组合预测并将其转换为正确的格式。