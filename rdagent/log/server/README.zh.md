# API

## A. 控制接口

### 1. /upload [POST]

#### 请求

- "scenario": 六个值之一
    1. "Finance Data Building"
    2. "Finance Data Building (Reports)"
    3. "Finance Model Implementation"
    4. "General Model Implementation"
    5. "Medical Model Implementation"
    6. "Data Science"
- "files": **2** 个场景需要此字段
    1. 在 "Finance Data Building (Reports)" 场景中，一个或多个 pdf 文件。
    2. 在 "General Model Implementation" 场景中，一个 pdf 文件或一个 pdf 链接，如 `https://arxiv.org/pdf/2210.09789`
- "competition": **Data Science** 场景需要此字段，75 个竞赛之一。
- "loops": RD-Agent 将自动停止前的循环次数（可选；如果未设置，将不会自动停止，必须手动停止）。
- "all_duration": RD-Agent 运行后自动停止的总时长（小时）。如果未设置，代理将继续运行直到手动停止或通过 "loops" 参数停止。

#### 响应

- "id": 唯一标识符字符串，例如 `/home/rdagent_log/data_science/competition_A/trace_1` 或 `/home/rdagent_log/finance/trace_1`，用于标记此 RD-Agent 运行生成的一系列日志。

### 2. /control [POST]

#### 请求

- "id": 标识符
- "action": 三个值之一
    1. "pause"
    2. "resume"
    3. "stop"

#### 响应

- "status": "success" / "error: ..."

### 3. /trace [POST]

返回后端为当前 id 生成的、**尚未返回给前端**的消息序列。

#### 请求

- "id": 标识符
- "all": True / False。True 表示将返回所有尚未提供给前端的消息；False 返回随机 1 到 10 条消息。在大多数情况下，应设置为 True。
- "reset": True / False。重置意味着 "尚未返回给前端" 的指针将设置回为此 id 生成的第一条消息，即从头返回。在大多数情况下，应设置为 False。

#### 响应

- [Messages](#b-messages) 列表

## B. 消息格式

### Research

一个循环中只有 **2** 条消息

1. hypothesis

```json
{
    "tag": "research.hypothesis",
    "timestamp": "<isoformat>",
    "loop_id": "1",
    "content": {
        "hypothesis": "...",
        "reason": "...",
        "component": "...", // 仅存在于 Data Science 场景
        "concise_reason": "...",
        "concise_justification": "...",
        "concise_observation": "...",
        "concise_knowledge": "...",
    }
}
```

2. tasks

```json
{
    "tag": "research.tasks",
    "timestamp": "<isoformat>",
    "loop_id": "1",
    "content": [    // 任务列表
        {
            "name": "...",
            "description": "...",
            "model_type": "...", // 仅存在于 "Finance Model Implementation"、"General Model Implementation"、"Medical Model Implementation" 或某些 "Data Science" 任务中
            "architecture": "...", // 同上
            "hyperparameters": "...", // 同上
        },
        {

        }
        //... 同上
    ]
}
```

### evolving

- 1 到 10 对消息（代码和反馈），每条消息由一个 "evo_id" 标识，表示进化轮次。
- 在 **Data Science** 场景中，每个进化轮次仅包含 **一个任务**，但该任务的 "codes" 可能包含 **多个代码文件**。
- 在其他场景中，每个进化轮次可能包含 **多个任务**，但每个任务的 "codes" 将仅包含 **一个代码文件**。

1. codes

```json
{
    "tag": "evolving.codes",
    "timestamp": "<isoformat>",
    "loop_id": "1",
    "evo_id": "0",
    "content": [ // 任务名称和代码列表
        {
            "evo_id": "0",
            "target_task_name": "task_1",
            "workspace": { // 一个或多个代码
                "a.py": "...<python codes>",
                "b.py": "...<python codes>",
                //...
            }
        },
        {
            "evo_id": "0",
            "target_task_name": "task_2",
            "workspace": {
                "a.py": "...<python codes>",
                //...
            }
        }
        //... 同上
    ]
}

{
    "tag": "evolving.codes",
    "timestamp": "<isoformat>",
    "loop_id": "1",
    "evo_id": "1",
    "content": [
        //... 同上
    ]
}
```

2. feedbacks

```json
{
    "tag": "evolving.feedbacks",
    "timestamp": "<isoformat>",
    "loop_id": "1",
    "evo_id": "0",
    "content": [ // 反馈列表
        {
            "evo_id": "0",
            "final_decision": "True", // True 或 False
            "execution": "...",
            "code": "...",
            "return_checking": "..."
        },
        //... 同上
    ]
}

{
    "tag": "evolving.codes",
    "timestamp": "<isoformat>",
    "loop_id": "1",
    "evo_id": "1",
    "content": [
        //... 同上
    ]
}
```

### feedback

下面的每个标签在每个循环中只出现一次。

1. config（仅存在于 "Finance Data Building"/"Finance Data Building (Reports)"/"Finance Model Implementation"）

```json
{
    "tag": "feedback.config",
    "timestamp": "<isoformat>",
    "loop_id": "1",
    "content": {
        "config": "a markdown string",
    }
}
```

2. return_chart（仅存在于 "Finance Data Building"/"Finance Data Building (Reports)"/"Finance Model Implementation"）

```json
{
    "tag": "feedback.return_chart",
    "timestamp": "<isoformat>",
    "loop_id": "1",
    "content": {
        "chart_html": "chart html codes string",
    }
}
```

3. metric

```json
{
    "tag": "feedback.metric",
    "timestamp": "<isoformat>",
    "loop_id": "1",
    "content": {
        "result": "{ \"<metric_name>\": <value>, ... }" // 包含指标名称及其对应值的 JSON 字符串。
    }
}
```

4. hypothesis_feedback

```json
{
    "tag": "feedback.hypothesis_feedback",
    "timestamp": "<isoformat>",
    "loop_id": "1",
    "content": {
        "decision": "True",
        "reason": "...",
        "exception": "...",
        "observations": "...", // 可能不存在
        "hypothesis_evaluation": "...", // 可能不存在
        "new_hypothesis": "...", // 可能不存在
    }
}
```

# TODO

## Session

- 如何继续。
- 显示并复制 trace_id（名称）？
- 

## Page

1. 移除 Medical，添加 Finance Whole Pipeline
2. 