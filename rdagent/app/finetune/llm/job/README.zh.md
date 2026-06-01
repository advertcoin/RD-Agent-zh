# FT 作业运行器

`run_ft_job.sh` 在一个作业目录下并行启动多个 FT-Agent 运行。它是多 GPU 机器的便捷辅助工具；对于单次运行，首选 `../README.md` 中的命令。

## 快速开始

从 RD-Agent 仓库根目录：

```bash
# 1. 首先准备正常的 FT-Agent 配置。
# 请参阅 rdagent/app/finetune/llm/README.md 获取所需的值。
cp .env rdagent/app/finetune/llm/job/.env

# 2. 准备任务定义。
cp rdagent/app/finetune/llm/job/tasks.json.example rdagent/app/finetune/llm/job/tasks.json

# 3. 运行作业。
bash rdagent/app/finetune/llm/job/run_ft_job.sh rdagent/app/finetune/llm/job/tasks.json
```

脚本为每个任务打开一个 tmux 窗口，并将日志写入 `log/<job_id>/` 下。

## 要求

- `jq`
- `tmux`
- `conda`
- RD-Agent conda 环境，默认名为 `rdagent`

如果您的 RD-Agent 环境有不同的名称，请在运行脚本之前设置 `CONDA_ENV_NAME` 或将其添加到 `job/.env`：

```bash
CONDA_ENV_NAME=my-rdagent-env bash rdagent/app/finetune/llm/job/run_ft_job.sh
```

FT 训练/评估后端仍由 `job/.env` 中的 `FT_Coder_CoSTEER_env_type` 控制。在 `docker` 模式下，作业运行器跳过训练和 OpenCompass 环境的 conda 就绪检查。在 `conda` 模式下，它在第一个任务初始化后等待 `llm_finetune` 和 `opencompass` 环境。

## 任务配置

`tasks.json` 包含一系列独立运行：

```json
{
  "tasks": [
    {
      "model": "Qwen/Qwen2.5-7B-Instruct",
      "benchmark": "aime25",
      "gpus": "0,1",
      "timeout": "12h"
    },
    {
      "model": "Qwen/Qwen2.5-7B-Instruct",
      "benchmark": "chemcotbench_mol_edit",
      "gpus": "2,3",
      "benchmark_description": "Molecule Editing - perform valid SMILES edits and return JSON: {\"output\": \"<valid SMILES>\"}."
    }
  ]
}
```

| 字段 | 必填 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `model` | 是 | - | Hugging Face 模型 ID。 |
| `benchmark` | 是 | - | 基准键，例如 `aime25` 或 `chemcotbench_mol_edit`。 |
| `gpus` | 否 | `"0"` | `CUDA_VISIBLE_DEVICES` 的值。 |
| `timeout` | 否 | `"12h"` | 传递给 FT-Agent 循环的挂钟预算。 |
| `port` | 否 | - | 可选的本地 API 端口；为该任务写入 `OPENAI_API_BASE=http://localhost:<port>`。 |
| `benchmark_description` | 否 | 来自 `scenarios.json` | 任务和输出格式描述。 |

如果省略 `benchmark_description`，运行器会在 `scenarios.json` 中查找它。

## 监控

```bash
tmux attach -t rdagent
tail -f log/<job_id>/*.log
streamlit run rdagent/app/finetune/llm/ui/app.py
```

在 Streamlit UI 中，选择生成的作业文件夹作为日志源。