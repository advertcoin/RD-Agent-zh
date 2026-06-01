# RepoAnalyzer

RepoAnalyzer 是一个 Python 工具，用于分析和汇总 Python 仓库的内容。它提供仓库结构的高级概述，包括目录结构的树形表示以及有关文件、类和函数的详细信息。

## 功能

- 生成仓库的树形结构
- 汇总整个仓库
- 调整摘要的详细程度
- 从特定文件中提取内容
- 分析 Python 文件中的类和函数


## 使用方法

### 基本用法

```python
from repo_utils import RepoAnalyzer

# 使用仓库路径初始化 RepoAnalyzer
repo_analyzer = RepoAnalyzer("/path/to/your/repo")

# 生成仓库摘要
summary = repo_analyzer.summarize_repo()
print(summary)

# 从特定文件中提取内容
highlighted_content = repo_analyzer.highlight(["file1.py", "file2.py"])
print(highlighted_content)
```

### 调整详细程度

您可以使用以下参数调整摘要的详细程度：

- `verbose_level`: 控制摘要的总体详细程度
  - 0: 最小（仅文件名）
  - 1: 默认（文件信息、类名、函数名）
  - 2+: 详细（包括类内的方法细节）
- `doc_str_level`: 控制文档字符串的包含（0-2）
- `sign_level`: 控制函数签名的包含（0-2）

示例：

```python
detailed_summary = repo_analyzer.summarize_repo(verbose_level=2, doc_str_level=1, sign_level=1)
print(detailed_summary)
```

## 示例输出

### 仓库摘要

```
Workspace Summary for my_project
========================================

Repository Structure:
my_project/
├── main.py
├── utils/
│   ├── helper.py
│   └── config.py
├── models/
│   ├── model_a.py
│   └── model_b.py

This workspace contains 5 Python files.

File 1 of 5:
File: main.py
----------------------------------------
This file contains 1 class and 2 top-level functions.

Class: MainApp
  Description: Main application class for the project.
  This class has 3 methods.

Function: setup_logging
  Accepts parameters: log_level
  Purpose: Configure the logging for the application.

Function: main
  Purpose: Entry point of the application.

...
```

### 文件高亮

```python
highlighted_content = repo_analyzer.highlight(["main.py"])
print(highlighted_content["main.py"])
```

这将打印 `main.py` 文件的全部内容。

## 关键组件

### RepoAnalyzer 类

提供仓库分析功能的主类。

#### 方法：

- `summarize_repo(verbose_level=1, doc_str_level=1, sign_level=1)`: 生成仓库的综合摘要，包括树形结构。
- `highlight(file_names)`: 提取并返回指定文件的内容。

### 树形结构

摘要现在包含仓库目录结构的可视化表示，使理解项目的整体组织变得更容易。