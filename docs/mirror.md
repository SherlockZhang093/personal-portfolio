# 类银项目 · 技术策划工具链

[返回作品集](../README.md) · [源码仓库](https://github.com/SherlockZhang093/mirror_test)

## 目标与证据范围

将招式参数、输入分支、剧情步骤与运行时调试组织成可编辑、可检查的工作流程。以下依据本地源码和文档核查，尚未在本轮启动 Unity 验证；个人分工、团队规模和工具使用成效待补充。

## 技能与连招

![连招编辑器](../类银/连招编辑器.png)

| 对象 | 源码确认的职责 | 配置价值 |
| --- | --- | --- |
| PlayerComboMove | 独立招式、倍率、动画速度与蓄力参数 | 同一动画可配置不同招式 |
| PlayerComboTransition | 输入条件、接续窗口、命中要求 | 明确招式衔接条件 |
| PlayerComboInputDecision | 监听起始帧、判定帧、点按/长按/无输入目标 | 表达输入分支与时机 |
| PlayerComboStateDecision | 状态分支与默认目标 | 根据玩家状态选择起手 |

![技能帧编辑器](../类银/技能帧编辑器.png)

截图展示动画预览、帧标记、攻击框开关、动作时间和目标反应配置。原始截图局部有系统通知遮挡，需补清晰录制。

### 蓄力设计：在同一动作中定格与释放

交接文档描述“播放重拳—在配置位置冻结—持续蓄力—松开后继续播放”。动画驱动源码通过 AnimationClipPlayable.SetSpeed 暂停和恢复动作，避免释放时从头播放。

当前数据包含 chargeHoldFrame，并保留隐藏的旧归一化时间字段。源码与交接文档存在版本差异，具体参数应以当前运行版本为准。

**连招演示**

https://github.com/user-attachments/assets/da3c7f2d-734f-46a3-a2cc-a0d9e541e4be

**蓄力演示**

https://github.com/user-attachments/assets/967ebe28-f708-46f8-8696-65d78ccb1019

[下载连招原文件](../类银/连招.mp4) · [下载蓄力原文件](../类银/蓄力.mp4)

建议补录：修改招式配置、调整分支、进入测试场景验证，并展示一次边界情况。

## 剧情编辑与校验

StoryNarrativeEditorWindow 使用序列化对象编辑剧情段落与触发器，提供场景保存和检查入口。StoryNarrativeValidator 具体检查：

- 剧情 ID 为空或重复。
- 段落没有步骤，或步骤为空。
- 镜头步骤缺少目标。
- 需要目标的新手引导未配置对象。
- 特定生命资源步骤目标无效。
- 中文字体源缺失或样例字符无法生成。

这一模块将配置错误变成明确反馈。后续补充编辑器截图，以及“错误配置—检查提示—修复成功”的演示。

## 运行时监控

![玩家监控](../类银/运行时监控.png)

截图与代码显示玩家状态观察、武器解锁与装备、能力开关及参数调整、恢复生命等入口。窗口监听 Play Mode 状态，对部分修改调用 Undo.RecordObject。运行时修改是否完整恢复，需专门测试后再作保证。

## 源码定位

以下为项目内路径。本地版本不保证与公开仓库一致；本轮只作静态核查。

- Assets/MirrorTrial/Scripts/Player/PlayerMoveComboGraph.cs
- Assets/MirrorTrial/Scripts/Player/PlayerAnimationDriver.cs
- Assets/MirrorTrial/Editor/PlayerInputComboEditorWindow.cs 及分部文件
- Assets/MirrorTrial/Editor/Level/StoryNarrativeEditorWindow.cs
- Assets/MirrorTrial/Editor/Level/StoryNarrativeValidator.cs
- Assets/MirrorTrial/Editor/Player/PlayerStatsMonitorWindow.cs
- Docs/Combat/CODEX_SKILL_EDITOR_HANDOFF.md

## 待补证据

个人职责与协作边界；完整工具工作流录像；真实错误定位案例；使用反馈；有记录支持的效率对比。
