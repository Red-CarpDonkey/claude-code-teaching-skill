# 演示模型生成规则

## 适用范围

发现者模式和工程师模式中使用。演示模型用于可视化抽象概念、展示算法动态、或让用户通过调参建立直觉。

## 核心原则

1. **生成前必须先询问用户**
2. **当场按需编写**，不预建模型库
3. **代码以清晰展示概念为首要标准**，不分层、不限制长度
4. **用户可修改的参数用醒目注释标记**

## 支持的语言

- **Python**：数学可视化（matplotlib）、算法演示、数据模拟
- **HTML**：交互式 UI 演示、前端概念、动画
- **MATLAB**：用户明确要求时使用

## 代码结构要求

### Python 演示模板

```python
"""
[概念名称] 交互演示
运行: python demo.py
"""
import matplotlib.pyplot as plt
import numpy as np

# ============================================================
# 用户可调参数区域 —— 修改以下参数观察变化
# ============================================================

PARAM_1 = 10        # [参数含义]
PARAM_2 = 0.5       # [参数含义]
SHOW_STEPS = True   # 是否显示中间步骤

# ============================================================
# 核心逻辑（非必要不修改）
# ============================================================

def core_algorithm(param1, param2):
    """[算法一句话描述]"""
    # 实现
    pass

def visualize(result):
    """[可视化描述]"""
    # 绘图
    pass

if __name__ == "__main__":
    result = core_algorithm(PARAM_1, PARAM_2)
    visualize(result)
```

### HTML 演示模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>[概念名称] 演示</title>
    <style>
        /* 简洁实用样式 */
    </style>
</head>
<body>
    <h1>[概念名称]</h1>
    <!-- ====== 用户可调参数区域 ====== -->
    <div class="controls">
        <label>参数1: <input type="range" id="param1" min="1" max="100" value="50"></label>
    </div>
    <!-- ====== 演示区域 ====== -->
    <canvas id="demo" width="600" height="400"></canvas>
    <script>
        // 核心逻辑
    </script>
</body>
</html>
```

## 生成时机与交互流程

1. Skill 判断当前概念需要可视化演示
2. 向用户说明："这个概念用代码演示会更直观。需要我生成一个（Python/HTML）演示程序吗？"
3. 用户确认后当场生成代码
4. 告知用户如何运行（如 `python demo.py`），并简要说明可调参数的含义
5. 用户运行后，引导用户调参观察变化

## 代码长度

不设硬性限制。唯一标准是让概念清晰可见。宁可 200 行清楚明白，不要 50 行晦涩难懂。
