---
name: obsidian-vassago
description: Create and edit Obsidian graph relation type configurations for the Vassago plugin. Use when working with semantic relationships, graph view customization, relation styles, or ob_relation directory files.
---

# Obsidian Vassago Plugin Skill

This skill enables Claude Code to create and edit valid Vassago plugin configurations for semantic graph relationships in Obsidian.

## Overview

Vassago is an Obsidian plugin that enhances the graph view by displaying typed, styled relationships between notes. It uses:
- Frontmatter properties to define relationships
- Configuration files in `ob_relation/` directory to style relationships
- PIXI.js for custom graph rendering

## Quick Start

使用 Vassago 插件的基本流程：

1. **创建关系配置文件**（如果还不存在）
   - 在 `<vault-root>/ob_relation/` 文件夹中创建关系类型配置文件（如 `supports.md`）
   - 使用下面的模板配置样式和属性

2. **在笔记中使用关系**
   - 在笔记的 frontmatter 中添加关系属性
   - 例如：`supports: "[[目标笔记]]"`

3. **在图谱视图中查看**
   - 打开 Obsidian 图谱视图
   - 关系会以配置的样式显示

## Relation Configuration Files

### File Location

```
<vault-root>/
└── ob_relation/
    ├── supports.md
    ├── refutes.md
    ├── derived_from.md
    ├── equivalent_to.md
    └── related_to.md
```

> [!IMPORTANT]
> **创建新关系类型**
> 
> 如果你需要使用的关系类型还不存在，需要在 `ob_relation/` 文件夹中创建对应的配置文件。
> 
> 例如，要使用 `causes` 关系：
> 1. 在 `<vault-root>/ob_relation/` 文件夹中创建 `causes.md` 文件
> 2. 按照下面的模板配置样式和属性
> 3. 保存后即可在笔记的 frontmatter 中使用 `causes: "[[目标笔记]]"`

### File Structure

Each relation type configuration file contains:
1. **Frontmatter** (YAML) - Style configuration with `$` prefix
2. **Body** (Markdown) - Documentation about the relation type

### Basic Template

```markdown
---
$color: "#4CAF50"
$shape: straight
$pattern: solid
$width: 2
$arrow: true
$direction: outgoing
$inverse: "supported_by"
$label: "支持"
$description: "表示A支持/证实B的观点"
---

# supports（支持）

当节点A的内容支持、证实或强化节点B时使用此关系。

## 使用示例

```yaml
---
supports: "[[目标笔记]]"
---
```

## Configuration Properties

### Required Properties

#### `$color` - Edge Color

```yaml
$color: "#4CAF50"    # Hex color value
```

**Common Colors:**
- `#4CAF50` - Green (support, positive)
- `#F44336` - Red (refute, negative)
- `#2196F3` - Blue (derivation, neutral)
- `#9C27B0` - Purple (equivalence)
- `#00BCD4` - Cyan (relation, association)
- `#FF9800` - Orange (causation)
- `#808080` - Gray (general)

#### `$shape` - Line Shape

```yaml
$shape: straight    # straight | curved
```

| Value | Effect | Use Case |
|-------|--------|----------|
| `straight` | Straight line | Strong, definite relationships |
| `curved` | Bezier curve | Loose associations, visual distinction |

#### `$pattern` - Line Pattern

```yaml
$pattern: solid    # solid | dashed
```

| Value | Effect | Use Case |
|-------|--------|----------|
| `solid` | Solid line ━━━ | Strong, directional relationships |
| `dashed` | Dashed line ┈┈┈ | Equivalence, symmetric relationships |

#### `$width` - Line Width

```yaml
$width: 2    # Number (pixels), recommended 1-4
```

| Value | Effect | Use Case |
|-------|--------|----------|
| `1` | Thin | Secondary relationships |
| `2` | Standard | General relationships (recommended) |
| `3` | Thick | Important relationships |
| `4` | Very thick | Core relationships |

#### `$arrow` - Arrow Display

```yaml
$arrow: true    # true | false
```

| Value | Effect | Use Case |
|-------|--------|----------|
| `true` | Show arrow | Directional relationships |
| `false` | No arrow | Symmetric, equivalence relationships |

#### `$direction` - Edge Direction

```yaml
$direction: outgoing    # outgoing | incoming | bidirectional
```

| Value | Effect | Example |
|-------|--------|---------|
| `outgoing` | Current note → Target | A supports B |
| `incoming` | Target → Current note | A derived from B (actually B → A) |
| `bidirectional` | A ↔ B | A equivalent to B |

### Optional Properties

#### `$inverse` - Inverse Relation

```yaml
$inverse: "supported_by"    # Name of the inverse relation type
```

Used to define paired relationships:
```yaml
# supports.md
$inverse: "supported_by"

# supported_by.md
$inverse: "supports"
```

#### `$label` - Display Label

```yaml
$label: "支持"    # Text shown in graph view
```

**Recommendations:**
- Keep it short (≤4 characters)
- Use clear, meaningful text
- Can be Chinese or English

#### `$description` - Description

```yaml
$description: "表示A支持/证实B的观点"    # Detailed description
```

## Line Type Combinations

### Supported Combinations

| Combination | shape | pattern | Visual | Use Case |
|-------------|-------|---------|--------|----------|
| **Straight + Solid** | `straight` | `solid` | ━━━ | Strong, causal, derivation |
| **Straight + Dashed** | `straight` | `dashed` | ┈┈┈ | Equivalence, symmetric |
| **Curved + Solid** | `curved` | `solid` | ╭─╮ | Loose association, related |
| **Curved + Dashed** | `curved` | `dashed` | ╭┈╮ | Not supported (uses curved + solid) |

### Combination Examples

#### Strong Relationship (High Certainty)
```yaml
$shape: straight
$pattern: solid
$arrow: true
```

#### Equivalence Relationship (Symmetric)
```yaml
$shape: straight
$pattern: dashed
$arrow: false
$direction: bidirectional
```

#### Loose Association (Uncertain)
```yaml
$shape: curved
$pattern: solid
$arrow: true
```

## Predefined Relation Types

### Basic Relations

```yaml
# supports.md
---
$color: "#4CAF50"
$shape: straight
$pattern: solid
$width: 2
$arrow: true
$direction: outgoing
$inverse: "supported_by"
$label: "支持"
---
```

```yaml
# refutes.md
---
$color: "#F44336"
$shape: straight
$pattern: solid
$width: 2
$arrow: true
$direction: outgoing
$inverse: "refuted_by"
$label: "反驳"
---
```

```yaml
# derived_from.md
---
$color: "#2196F3"
$shape: straight
$pattern: solid
$width: 2
$arrow: true
$direction: incoming
$inverse: "derives"
$label: "派生自"
---
```

```yaml
# equivalent_to.md
---
$color: "#9C27B0"
$shape: straight
$pattern: dashed
$width: 2
$arrow: false
$direction: bidirectional
$label: "等价于"
---
```

```yaml
# related_to.md
---
$color: "#00BCD4"
$shape: curved
$pattern: solid
$width: 2
$arrow: true
$direction: bidirectional
$label: "相关"
---
```

## Using Relations in Notes

### In Frontmatter (Recommended)

```yaml
---
supports: "[[热力学第二定律]]"
refutes: "[[永动机理论]]"
derived_from:
  - "[[玻尔兹曼熵]]"
  - "[[信息论]]"
equivalent_to: "[[信息熵]]"
---
```

### Single Target

```yaml
---
supports: "[[Target Note]]"
---
```

### Multiple Targets (Array)

```yaml
---
derived_from:
  - "[[Source 1]]"
  - "[[Source 2]]"
  - "[[Source 3]]"
---
```

### Multiple Relation Types

```yaml
---
supports: "[[Theory A]]"
refutes: "[[Theory B]]"
derived_from: "[[Concept C]]"
related_to:
  - "[[Topic 1]]"
  - "[[Topic 2]]"
---
```

## Complete Configuration Example

```markdown
---
$color: "#4CAF50"
$shape: straight
$pattern: solid
$width: 2
$arrow: true
$direction: outgoing
$inverse: "supported_by"
$label: "支持"
$description: "表示A支持/证实B的观点"
---

# supports（支持）

当节点A的内容支持、证实或强化节点B时使用此关系。

## 使用示例

在笔记的 frontmatter 中定义：

```yaml
---
supports: "[[目标笔记]]"
---
```

或多个目标：

```yaml
---
supports:
  - "[[理论A]]"
  - "[[理论B]]"
---
```

## 语义说明

- **方向**: 从当前笔记指向目标笔记
- **反向关系**: supported_by（被支持）
- **颜色**: 绿色 (#4CAF50)
- **形状**: 直线（straight）
- **图案**: 实线（solid）
```

## Best Practices

### Naming Conventions

- **File names**: Use lowercase with underscores: `supports.md`, `derived_from.md`
- **Labels**: Use short, clear text (Chinese or English)
- **Inverse relations**: Create paired configuration files

### Color Selection

- **Semantic**: Choose colors that match the meaning
  - Green → Positive, support
  - Red → Negative, refute
  - Blue → Neutral, derivation
- **Contrast**: Ensure visibility in both dark and light themes
- **Consistency**: Use similar colors for related relation types

### Shape and Pattern

- **Straight**: For clear, strong relationships
- **Curved**: For loose, weak relationships
- **Solid**: For directional relationships
- **Dashed**: For symmetric or equivalence relationships

### Direction Settings

- **outgoing**: Most relationships (A affects B)
- **incoming**: Reverse relationships (A comes from B)
- **bidirectional**: Symmetric relationships (A equals B)

## Important Notes

### $ Prefix Requirement

All configuration properties MUST use the `$` prefix to prevent conflicts with Obsidian internal variables.

**Correct:**
```yaml
$color: "#4CAF50"
$shape: straight
```

**Incorrect:**
```yaml
color: "#4CAF50"    # Will not work!
shape: straight     # Will not work!
```

## References

- [Vassago Plugin Documentation](../README.md)
- [边的样式配置说明](../docs/边的样式配置说明.md)
- [图结构笔记插件设计](../docs/图结构笔记插件设计.md)
- [笔记使用指南](../docs/笔记使用指南.md)
