# 介绍

!!!!!此开发容器不面向公共开发使用！！！！！
!!!!!如果您只是公共开源用户，请不要使用它。！！！！！

# 运行开发容器的步骤（仅供内部使用）

前提条件（这就是为什么此开发容器不面向公共使用的原因）：

- 确保您本地有 `rdagentappregistry.azurecr.io/rd-agent-mle:20250623` 镜像，并且您的 IDE 中安装了 DevContainer
- Kaggle 数据集位于 `/home/shared/RD-Agent/kaggle`

1. 打开项目并选择 "Open In DevContainer"
2. 设置您的 Kaggle Key（请勿共享；其他内部 URL 在配置文件中是硬编码的）

```bash
export KAGGLE_USERNAME=
export KAGGLE_KEY=
```

3. 运行：python rdagent/app/data_science/loop.py --competition nomad2018-predict-transparent-conductors


# 附加说明
- 请在 VS Code 中安装和使用此 Dev Container。
- 您**必须远程打开 VS Code 并在运行 DevContainer 配置（`.devcontainer/devcontainer.json`）之前进入 `RD-Agent` 目录**。否则，工作区和路径映射将无法按预期工作。
- 要在 VS Code 中正确打开 DevContainer：
  1. 远程连接到机器并在 VS Code 中打开 `RD-Agent` 文件夹。
  2. 按 `Ctrl+Shift+P`（或在 Mac 上按 `Cmd+Shift+P`），输入并选择 **"Dev Containers: Reopen in Container"**。



# 如何在 DevContainer 中评分您的提交

1. 将您的提交文件保存在 `./sumission.csv`

2. 运行评估
DS_COMPETITION=<your competition name>
conda run -n mlebench  mlebench grade-sample submission.csv $DS_COMPETITION --data-dir /tmp/kaggle/zip_files/