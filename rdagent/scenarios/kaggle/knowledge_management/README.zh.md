## 使用方法

本文件夹基于 Kaggle 竞赛实现了一个使用 RAG 的知识库。它允许您将 Kaggle 竞赛经验存储到知识库中，以及存储来自 RD-Agent 的实验经验。

1. 首先，运行 `extract_knowledge.py` 中的 `main` 函数生成知识库（JSON 格式）。
2. 然后，在 `vector_base.py` 中创建向量库并保存。
3. 最后，在您的 `.env` 文件中添加字段 `KG_RAG_PATH="xxx.pkl"`（保存的向量库路径）。