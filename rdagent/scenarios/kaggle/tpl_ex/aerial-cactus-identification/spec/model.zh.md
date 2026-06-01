## 模型工作流程

- 实现一个管理模型工作流程的函数，具有以下签名：

```python
def model_workflow(X: np.ndarray, y: np.ndarray, val_X: np.ndarray = None, val_y: np.ndarray = None, test_X: np.ndarray = None, **hyper_params) -> tuple[np.ndarray | None, np.ndarray | None, dict]:
    """
    管理机器学习模型的工作流程，包括训练、验证
    也包括测试和验证的推理

    - 如果测试/验证存在，输出它们的推理结果
    - 如果存在超参数，遵循超参数
        - 超参数至少包含 <early stop round>。代码必须检查是否给出并使用它。
        - 返回的超参数应与输入对齐（除了新生成的 early stop）
    - 如果不存在超参数，返回用于重新训练的超参数。超参数应包含 <early stop round>
    - 如果验证存在，添加 <early stop round> 来更新超参数


    参数
    ----------
    X : np.ndarray
        训练数据特征。
    y : np.ndarray
        训练数据标签。
    val_X : np.ndarray, 可选
        验证数据特征。
    val_y : np.ndarray, 可选
        验证数据标签。
    test_X : np.ndarray, 可选
        测试数据特征。
    **hyper_params
        模型的额外超参数。

    返回
    -------
    tuple[np.ndarray | None, np.ndarray | None, dict]
        验证数据的预测，测试数据的预测
    """
```

- 在本任务中，输入（训练、验证和测试的 X）的形状应为 (num_samples, height, width, channels)。

- 在本任务中，输出的形状应为 (num_samples, num_class)，因为 num_class = 1。

- 该函数应处理数据增强、模型创建、训练和预测。