# State.json 完整格式定义

## 顶层字段

```json
{
  "topic": "用户当前学习的主题",
  "mode": "discoverer | engineer | dialoguer | deep_diver",
  "phase": "diagnosis | teaching",
  "vault": {
    "mode_folder": "10-理论学习",
    "topic_folder": "[模式文件夹]/[主题名]",
    "blackboard_file": "[主题名]/[板书]-实时笔记.md",
    "index_file": "00-总览仪表盘/索引.md",
    "last_card_file": "上一个拆分的卡片路径"
  },
  "split_cards": [
    {"file": "[知识点]-[名称].md", "title": "[名称]", "created_at": "..."}
  ],
  "current_node": "当前正在进行的教学节点名",
  "completed_nodes": [
    {"name": "节点名", "completed_at": "2026-05-28"}
  ],
  "spiral_queue": [
    {"node": "节点名", "round": 2, "after_cycle": 3}
  ],
  "question_counts": {"知识点": 提问次数},
  "cycle_count": 0,
  "deferred_questions": [
    {
      "question": "用户原始问题",
      "deferred_from_meta_stage": "II",
      "target_meta_stage": "III",
      "timestamp": "2026-06-08T...",
      "status": "pending | answered"
    }
  ],
  "discoverer": { ... },
  "engineer": { ... },
  "dialoguer": { ... },
  "observer": { ... },
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
  "ripple_branch_chosen": "用户在阶段六选择的涟漪分支方向",
  "textbook": {"title": "教材名", "current_chapter": "Ch3", "chapters_completed": ["Ch1", "Ch2"]}
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
  "extension_chosen": "用户在阶段六选择的拓展选题",
  "textbook": {"title": "教材名", "current_chapter": "Ch3", "chapters_completed": ["Ch1", "Ch2"]}
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
  "thought_history_position": null,
  "textbook": {"title": "教材名", "current_chapter": "Ch3", "chapters_completed": ["Ch1", "Ch2"]}
}
```

## 观察者模式字段 (observer)

```json
{
  "stage": 1,
  "phenomenon": "用户在阶段一面对的具体异常现象",
  "user_variables": ["用户识别的变量列表"],
  "user_hypothesis": "用户在阶段三提出的因果假设",
  "competing_theory": "阶段四引入的竞争理论名称",
  "model_application_result": "阶段五模型应用到新现象的结果",
  "textbook": {"title": "教材名", "current_chapter": "Ch3", "chapters_completed": ["Ch1", "Ch2"]}
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
  "creative_work": "用户完成的内化作品路径或描述",
  "textbook": {"title": "经典名/注疏名", "current_chapter": "篇三", "chapters_completed": ["篇一", "篇二"]}
}
```

## 旧版状态迁移

旧 state.json（topic/type/phase）到新格式的迁移映射：
- `type: "tool"` → `mode: "engineer"`，`type: "theory"` → `mode: "discoverer"`
- 首次启动时自动检测并升级

## vault 追踪字段说明

### vault 对象

| 字段 | 说明 |
|------|------|
| `mode_folder` | 当前模式对应的顶层文件夹（10-理论学习/20-工具栈/30-文科论题/40-经典专著） |
| `topic_folder` | 当前主题的子文件夹完整路径 |
| `blackboard_file` | 当前板书文件路径，每次回复追加写入 |
| `index_file` | 索引文件路径 |
| `last_card_file` | 最近一张拆分出的卡片路径，用于设置下一张的 `previous` 链接 |

### split_cards 数组

每次分支拆分时追加一条记录，用于追踪学习链。

```json
{
  "file": "[知识点]-欧拉回路.md",
  "title": "欧拉回路",
  "created_at": "2026-06-03T12:30:00"
}
```
