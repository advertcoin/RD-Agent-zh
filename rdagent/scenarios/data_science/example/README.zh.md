# R&D-Agent 数据科学管道中自定义数据的详细说明

R&D-Agent 数据科学管道支持对 Kaggle 平台上托管的竞赛进行自动化研发优化，以及**自定义用户定义的数据集**。

具体来说，您需要按照提供的示例类似的结构准备文件。这里，我们使用 `arf-12-hours-prediction-task` 数据集作为示例。

## arf-12-hours-prediction-task 简介

> 急性呼吸衰竭（ARF）是一种危及生命的疾病，通常在危重病人中迅速发展。在重症监护病房（ICU）中准确早期预测 ARF 对于及时进行临床干预和有效分配资源至关重要。在此任务中，您需要构建一个机器学习模型，使用多变量临床时间序列数据预测患者在未来**12小时内**是否会发生 ARF。
>
> 数据集已从电子健康记录（EHRs）中提取，并通过**FIDDLE**管道进行预处理，为每位患者生成结构化的时间特征。

## 示例文件夹结构

* `source_data`（**必需**）

  * `arf-12-hours-prediction-task`（任务名称，**必需**）

    * `prepare.py` 用于数据预处理，将原始数据分为：*训练数据*、*测试数据*、*格式化提交文件*和*标准答案文件*。

  * `playground-series-s4e9`（任务名称，**必需**）

    * `prepare.py`（**必需**）：用于数据预处理，将原始数据分为：*训练数据*、*测试数据*、*格式化提交文件*和*标准答案文件*。

  注意：由于原始数据较大，我们不在此项目中显示原始数据，如果您想查看原始数据，可以通过底部链接下载完整数据集。

* `arf-12-hours-prediction-task`（任务名称）

  * `description.md`（**必需**）：任务的详细描述，包括*任务描述*、*目标*、*数据描述*、*数据使用注意事项*、*建模*、*评估*和*提交格式*等部分。

  * `sample.py`（**可选**）：用于调试目的对数据集进行采样的 Python 脚本。如果未提供，将使用 R&D-Agent 中的默认采样逻辑。请参阅 `rdagent/scenarios/data_science/debug/data.py` 中的 `create_debug_data` 函数。

* `playground-series-s4e9`（任务名称）

  * `description.md`（**必需**）：任务的详细描述，包括*任务描述*、*目标*、*评估*、*数据描述*和*提交格式*等部分。

* `eval`（**可选**）

  * `arf-12-hours-prediction-task`（任务名称，**可选**）

    * `grade.py`：计算测试数据集上的任务分数。
    * `valid.py`：检查生成的 `submission.csv` 文件的有效性。

  * `playground-series-s4e9`（任务名称，**可选**）

    * `grade.py`：计算测试数据集上的任务分数。
    * `valid.py`：检查生成的 `submission.csv` 文件的有效性。

  注意：如果您忽略测试集分数，则不需要创建 `eval` 文件夹。

---

`arf-12-hours-prediction-task` 的完整数据集文件夹可以从[这里](https://github.com/SunsetWolf/rdagent_resource/releases/download/ds_data/arf-12-hours-prediction-task.zip)下载。

`arf-12-hours-prediction-task` 的原始数据集来自 PhysioNet。您可以在[PhysioNet](https://physionet.org/)申请账户，然后请求访问 FIDDLE 预处理数据：[FIDDLE Dataset](https://physionet.org/content/mimic-eicu-fiddle-feature/1.0.0/)。

---

`playground-series-s4e9` 的完整数据集文件夹可以从[这里](https://github.com/SunsetWolf/rdagent_resource/releases/download/ds_data/playground-series-s4e9.zip)下载。

`playground-series-s4e9` 的原始数据集来自 Kaggle。您可以在[Kaggle](https://www.kaggle.com/)申请账户，然后请求访问[竞赛数据集](https://www.kaggle.com/competitions/playground-series-s4e9/data)。

---

**注意：**有关数据集的更多信息，请参阅[文档](https://rdagent.readthedocs.io/en/latest/scens/data_science.html)。