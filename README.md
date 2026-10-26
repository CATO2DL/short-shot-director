# short-shot-director · 短镜头导演

设计一个场景、一段明确动作、一种主要相机运动的 AI 短镜头，输出可直接使用的图片与视频提示词、按需素材表和局部修复方案。接受一句想法、图片、视频参考或 Visora 的 `design-spec.md`。

适用于巨构、梦核、东方幻想、自然景观、人物氛围、产品展示和单人表演。通常是几秒的连续镜头，时长按用户需求与生成工具决定。不承担完整短剧、多场景剧情、多人物连续性系统、复杂剪辑工程或默认三维建模。

## 下载与安装

- [GitHub 源码](https://github.com/CATO2DL/short-shot-director)
- [v1.0.0 安装包](https://github.com/CATO2DL/short-shot-director/releases/download/v1.0.0/short-shot-director-v1.0.0.zip)
- [发布页与 SHA-256 校验文件](https://github.com/CATO2DL/short-shot-director/releases/tag/v1.0.0)

ZIP 内有顶层 `short-shot-director/` 目录。完整保留其中的 `SKILL.md`、`references/`、`templates/` 等文件；不要只复制入口文件。它是独立 Agent Skill，没有可执行安装程序，不需要 API 密钥或视频生成服务才能使用。

### Codex 本地安装

下载并解压 ZIP 后，把整个 `short-shot-director` 文件夹放到 `~/.agents/skills/`。项目专用安装可放到该项目的 `.agents/skills/`。也可以在终端克隆：

```sh
mkdir -p "$HOME/.agents/skills"
git clone --branch v1.0.0 --depth 1 https://github.com/CATO2DL/short-shot-director.git "$HOME/.agents/skills/short-shot-director"
```

以上命令要求目标目录尚不存在；已有同名技能时先比较版本，避免覆盖自己的修改。使用 ZIP 时先在下载目录解压，再将文件夹移入目标目录，保持 `short-shot-director/SKILL.md` 只有一层技能目录。

安装后请 Codex 使用 `$short-shot-director`。若未被发现，重启 Codex 再检查目录。目录与发现方式依据 [OpenAI 官方技能文档](https://learn.chatgpt.com/docs/build-skills)；其他支持 Agent Skills 的宿主请使用该宿主的本地技能目录。`agents/openai.yaml` 仅是可选的 Codex 展示元数据。此仓库不表示已提交到任何插件商店。

## 使用

```text
使用 $short-shot-director，设计一个巨墙下只有小背影的镜头。
相机原地向上摇，最后仍看不到墙顶。只给提示词，不生成。
```

```text
使用 $short-shot-director，读取我提供的 design-spec.md。
保留既定风格、配色和材质，设计云海宫殿的缓慢横移镜头。
```

```text
把 S01 改成“镜头不要动，人从左边走过去”。
保留外观和配色，更新完整提示词及受影响的素材与结束构图。
```

只描述想法也能开始；缺少关键需求时每轮最多问两个问题。其余选择会标为设计建议。已有明确参考时直接继承，不重复要求风格选择。

## 交付内容

每个镜头一个 Markdown 文件，包含 11 节：概念、画面与运镜参数、素材表、完整首帧提示词、必要的尾帧／补充视角提示词、完整视频提示词、参考绑定、工具适用的排除要求、三阶段时间变化、验收清单、失败与局部修复。

未知平台时给工具中立版本；确认功能后才输出该平台相应模式的执行说明。参数、焦段、色值与比例是视觉设计建议，不是模型的精度保证。默认只交付方案；用户要求生成时再使用实际可用且已授权的工具。

提供 Visora 文件时另建镜头文档，保留原 `design-spec.md`。这个技能也能独立使用，不依赖 Visora 的安装或服务。

## 文件导航

```text
short-shot-director/
  SKILL.md
  README.md
  LICENSE
  agents/openai.yaml
  references/
    camera-movement.md
    reference-planning.md
    prompt-writing.md
    review-and-repair.md
  templates/shot.md
  examples/
    01-megastructure-tilt-up.md
    02-cloud-palace-tracking.md
    03-character-turn.md
    04-product-orbit.md
```

从 [技能入口](SKILL.md) 或 [镜头模板](templates/shot.md) 开始。四个完整示例：

| 示例 | 主运动 | 素材判断 |
| --- | --- | --- |
| [巨墙下的小人物](examples/01-megastructure-tilt-up.md) | 固定位置向上摇 | 小背影不需脸部多视图；尾构图可选 |
| [云海宫殿](examples/02-cloud-palace-tracking.md) | 缓慢向右横移 | 短距离未绕过边缘，不默认补背面 |
| [单人缓慢转头](examples/03-character-turn.md) | 固定机位 | 选定主参考后，按需要派生同身份侧面 |
| [产品小幅环绕](examples/04-product-orbit.md) | 正面转向前侧面 | 评估新可见侧面；不自动索取完整背面 |

所有示例均为已填写的制作方案，图片和视频均待生成，视觉效果未验证。素材表中的空路径表示实际文件尚不存在。验收清单是未来生成后的检查条件，不是通过记录。

## 许可证

技能源码、文档、模板与示例按 [MIT License](LICENSE) 开源。用户另行提供的参考图片、视频和品牌素材仍遵循其原有权利与许可。
