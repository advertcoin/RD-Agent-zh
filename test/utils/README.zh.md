# 🐳 运行 Docker & Qlib
---

## 📄 描述
本指南说明如何在 RD-Agent 仓库中运行位于 `test/utils/test_env.py` 的 Qlib Docker 测试文件。

---

## 🚀 运行说明

### 1. 安装所需的 Python 库
- 确保安装了 `docker` Python 库：
    ```sh
    pip install docker
    ```

### 2. 运行测试脚本
- 执行测试脚本来验证 Docker 环境设置：
    ```sh
    python test/utils/test_env.py
    ```

### 故障排除
- **PermissionError: [Errno 13] Permission denied.**
    > 当当前用户没有访问 Docker socket 的必要权限时会发生此错误。要解决此问题，请按照以下步骤操作：

1. **将当前用户添加到 `docker` 组**
Docker 需要 root 或 `docker` 组用户权限才能访问 Docker socket。将当前用户添加到 `docker` 组：
    ```sh
    sudo usermod -aG docker $USER
    ```

2. **刷新组更改**
要应用组更改，请注销并重新登录，或使用以下命令：
    ```sh
    newgrp docker
    ```

3. **验证 Docker 访问**
运行以下命令以确保可以访问 Docker：
    ```sh
    docker run hello-world
    ```

4. **重新运行测试脚本**
    完成这些步骤后，重新运行测试脚本：
    ```sh
    python test/utils/test_env.py
    ```
---
## 🛠️ Qlib Docker 函数框架详细说明

这里提供 Qlib Docker 框架中具体函数的概述、它们的用途以及调用示例。

### `env.py` 中的 QTDockerEnv 类

`QTDockerEnv` 类负责为 Qlib 实验设置和运行 Docker 环境。

#### 方法：

1. **prepare()**
   - **用途**：准备 Docker 环境以运行实验。这包括在必要时构建 Docker 镜像。
   - **示例**：
     ```python
     qtde = QTDockerEnv()
     qtde.prepare()
     ```

2. **run(local_path: str, entry: str) -> str**
   - **用途**：在准备好的 Docker 环境中运行指定的入口点（例如配置文件）。
   - **参数**：
     - `local_path`: 要挂载到 Docker 容器中的本地目录路径。
     - `entry`: 要在 Docker 容器内运行的命令或入口点。
   - **返回**：Docker 容器的 stdout 输出。
   - **示例**：
     ```python
     result = qtde.run(local_path="/path/to/env_tpl", entry="qrun conf.yaml")
     ```
---
### 📊 预期输出

成功执行后，测试脚本将生成基准收益和各种风险指标的分析结果。预期输出应类似于：

```
'The following are analysis results of benchmark return (1 day).'
risk
mean               0.000477
std                0.012295
annualized_return  0.113561
information_ratio  0.598699
max_drawdown      -0.370479

'The following are analysis results of the excess return without cost (1 day).'
risk
mean               0.000530
std                0.005718
annualized_return  0.126029
information_ratio  1.428574
max_drawdown      -0.072310

'The following are analysis results of the excess return with cost (1 day).'
risk
mean               0.000339
std                0.005717
annualized_return  0.080654
information_ratio  0.914486
max_drawdown      -0.086083

'The following are analysis results of indicators (1 day).'
value
ffr    1.0
pa     0.0
pos    0.0
```

按照这些步骤并使用提供的函数，您应该能够运行 Qlib Docker 测试并获得预期的分析结果。