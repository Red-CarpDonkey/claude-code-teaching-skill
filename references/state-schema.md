# State.json 完整格式定义

## 顶层字段

```json
{
  "topic": "用户当前学习的主题",
  "mode": "discoverer | engineer | dialoguer | deep_diver",
  "phase": "diagnosis | teaching",
  "current_node": "当前正在进行的教学节点名",
  "completed_nodes": [
    {"name": "节点名", "completed_at": "2026-05-28"}
  ],
  "spiral_queue": [
    {"node": "节点名", "round": 2, "after_cycle": 3}
  ],
  "question_counts": {"知识点": 提问次数},
  "cycle_count": 0,
  "discoverer": { ... },
  "engineer": { ... },
  "dialoguer": { ... },
  "deep_diver": { ... }
}
```

## 发现者模式字段 (discoverer)

```json
{
  "stage": 1,
  "hypothesis": "用户在阶段二提出的初始假设",
  "chosen_path": "用户在阶段四选择的路径",
  "preknowledge_gaps": ["已识别但用户选择不展开的前置概念"],
  "proof_status": "not_started | in_progress | complete",
  "ripple_branch_chosen": "用户在阶段六选择的涟漪分支方向"
}
```

## 工程师模式字段 (engineer)

```json
{
  "stage": 1,
  "user_design_thoughts": "用户在阶段二提出的原始设计思路",
  "milestones_completed": ["已完成的代码里程碑名称"],
  "current_code_path": "当前项目代码的相对路径",
  "refactor_applied": false,
  "extension_chosen": "用户在阶段六选择的拓展选题"
}
```

## 对话者模式字段 (dialoguer)

```json
{
  "stage": 1,
  "initial_position": "用户在阶段二给出的初始立场",
  "encountered_sages": ["[先贤A]", "[先贤B]"],
  "debate_rounds": {"[先贤A]": N, "[先贤B]": N},
  "final_position": "用户在阶段六给出的最终立场",
  "thought_history_position": null
}
```

## 深度潜入者模式字段 (deep_diver)

```json
{
  "stage": 1,
  "classic": "正在研读的经典名称",
  "current_passage": "当前原文片段的章节位置",
  "user_confusions": ["素读阶段暴露的困惑列表"],
  "annotations_chosen": ["选择的注疏解释"],
  "dialogues_generated": 0,
  "creative_work": "用户完成的内化作品路径或描述"
}
```

## 旧版状态迁移

旧 state.json（topic/type/phase）到新格式的迁移映射：
- `type: "tool"` → `mode: "engineer"`，`type: "theory"` → `mode: "discoverer"`
- 首次启动时自动检测并升级
