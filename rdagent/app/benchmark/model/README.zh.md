# 任务

## 任务提取
从论文到任务。
```bash
# python rdagent/app/model_implementation/task_extraction.py
# It may based on rdagent/document_reader/document_reader.py
python rdagent/components/task_implementation/model_implementation/task_extraction.py ./PaperImpBench/raw_paper/
```

## 完整工作流程
从论文到实现
``` bash
# Similar to
# rdagent/app/factor_extraction_and_implementation/factor_extract_and_implement.py
```

## 论文基准测试
```bash
# TODO: it does not work well now.
python rdagent/app/model_implementation/eval.py
```

TODO:
- 创建合理的基准测试
  - 使用统一的输入
  - 手动创建任务
- 创建合理的评估指标

## 进化