# short-shot-director · 短镜头导演

支持直接带图，也支持把视频风格／照片提示词转成可供选择的图片，再整理给视频生成 AI 的素材包。先询问 n 组候选、每组 m 个视角，分别选择图片模型与视频模型；生成并展示图片，等你选定后，创建只含必要镜头图片与提示词的精简文件夹与 ZIP。也支持一句想法、已有图片、视频参考或 Visora 的 `design-spec.md`。

适用于巨构、梦核、东方幻想、自然景观、人物氛围、产品展示和单人表演。通常是几秒的连续镜头，时长按用户需求与生成工具决定。不承担完整短剧、多场景剧情、多人物连续性系统、复杂剪辑工程或默认三维建模。

## 下载与安装

- [GitHub 源码](https://github.com/CATO2DL/short-shot-director)
- [v1.3.0 安装包](https://github.com/CATO2DL/short-shot-director/releases/download/v1.3.0/short-shot-director-v1.3.0.zip)
- [发布页与 SHA-256 校验文件](https://github.com/CATO2DL/short-shot-director/releases/tag/v1.3.0)

ZIP 内有顶层 `short-shot-director/` 目录。完整保留其中的 `SKILL.md`、`references/`、`templates/` 等文件；不要只复制入口文件。它是独立 Agent Skill，没有可执行安装程序，不需要 API 密钥或视频生成服务才能使用。

### Codex 本地安装

下载并解压 ZIP 后，把整个 `short-shot-director` 文件夹放到 `~/.agents/skills/`。项目专用安装可放到该项目的 `.agents/skills/`。也可以在终端克隆：

```sh
mkdir -p "$HOME/.agents/skills"
git clone --branch v1.3.0 --depth 1 https://github.com/CATO2DL/short-shot-director.git "$HOME/.agents/skills/short-shot-director"
```

以上命令要求目标目录尚不存在；已有同名技能时先比较版本，避免覆盖自己的修改。使用 ZIP 时先在下载目录解压，再将文件夹移入目标目录，保持 `short-shot-director/SKILL.md` 只有一层技能目录。

安装后请 Codex 使用 `$short-shot-director`。若未被发现，重启 Codex 再检查目录。目录与发现方式依据 [OpenAI 官方技能文档](https://learn.chatgpt.com/docs/build-skills)；其他支持 Agent Skills 的宿主请使用该宿主的本地技能目录。`agents/openai.yaml` 仅是可选的 Codex 展示元数据。此仓库不表示已提交到任何插件商店。

## 使用

### 方式一：自带图片

```text
使用 $short-shot-director。用我提供的这张图做首帧，设计一个缓慢横移镜头。
问我目标视频模型，然后把原图和完整视频提示词整理成素材包，不需要重新生图。
```

一张或多张图都可以。单张且已明确要使用时直接作为首帧；多张则确认首帧、尾帧和辅助参考的角色。不强制问 n×m，也不要求知道原图片模型或原始提示词。只有需要补图／改图时才询问图片模型；未知来源如实记录，原图保留。

### 方式二：先生成候选，再选择

```text
使用 $short-shot-director。我的风格提示词是：东方幻想、云海宫殿、清晨暖光。
先问我生成几组、每组几个视角，分别问图片模型和视频模型。
生成图片展示给我，等我选好后，把选定图片与提示词整理成视频素材包。
```

新版流程：

1. 确定是否先生图、n×m 的数量和视角；m 包含主视角。
2. 分别询问图片模型与视频模型，每轮最多两个问题，已有答案不再重复问。
3. 每组先生成主参考，再派生该组其余视角，记录每张图片实际模型。
4. 展示真实图片与组号、视角号，等待你选定候选组和首帧。
5. 创建只含本镜头必要图片与完整视频提示词的文件夹和 ZIP；需要复用图片提示词时才附上。

图片模型和视频模型不会混为一谈；工具未披露模型名时如实记录。没有可用图片工具时只能交付待生成计划，不能假装生成或选图完成。目标视频能力未核实时，包会标为未适配草案。交付素材包不会自动调用视频生成接口。

保留只写提示词的用法：

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

默认素材包只保留必要镜头图片和可直接使用的提示词，最小结构为：

```text
S01-shot/
  start.jpg
  video.txt
```

需要且工具支持时才加入尾帧或必要参考图；只有图片提示词仍需复用时才加 image-prompts.txt。无用角度、候选图、README、manifest、来源说明、选择记录、日志、旧版本和完整镜头文档都不放进去。模型和输入角色等执行必要信息用几行写在 video.txt 中，ZIP 内容与文件夹一致。

用户只要镜头方案或明确要求完整设计文档时，仍可按 11 节镜头模板交付 Markdown。下方技能源码中的 README、指南与模板用于安装和指导技能，与生成的镜头素材包分开。

未知平台时给工具中立版本；确认功能后才输出该平台相应模式的执行说明。参数、焦段、色值与比例是视觉设计建议，不是模型的精度保证。收到风格提示词时先询问是否生成候选；只有用户选择生成，才使用实际可用且已授权的工具。明确只要提示词时直接交付方案。

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
    candidate-selection.md
    handoff-package.md
    user-images.md
  templates/
    shot.md
  examples/
    01-megastructure-tilt-up.md
    02-cloud-palace-tracking.md
    03-character-turn.md
    04-product-orbit.md
    05-candidate-to-handoff.md
    06-user-provided-images.md
```

从 [技能入口](SKILL.md) 或 [镜头模板](templates/shot.md) 开始。四个完整示例：

| 示例 | 主运动 | 素材判断 |
| --- | --- | --- |
| [巨墙下的小人物](examples/01-megastructure-tilt-up.md) | 固定位置向上摇 | 小背影不需脸部多视图；尾构图可选 |
| [云海宫殿](examples/02-cloud-palace-tracking.md) | 缓慢向右横移 | 短距离未绕过边缘，不默认补背面 |
| [单人缓慢转头](examples/03-character-turn.md) | 固定机位 | 选定主参考后，按需要派生同身份侧面 |
| [产品小幅环绕](examples/04-product-orbit.md) | 正面转向前侧面 | 评估新可见侧面；不自动索取完整背面 |

新增 [两组、每组三视角的交互示例](examples/05-candidate-to-handoff.md)，说明模型询问、等待选择与素材包交付。详见 [候选流程](references/candidate-selection.md) 和 [文件夹规范](references/handoff-package.md)。

另见 [自带图片流程](references/user-images.md) 和 [单图、多图、混合补图示例](examples/06-user-provided-images.md)。

四个镜头示例均为已填写的制作方案，图片和视频均待生成，视觉效果未验证。素材表中的空路径表示实际文件尚不存在。验收清单是未来生成后的检查条件，不是通过记录。

## 许可证

技能源码、文档、模板与示例按 [MIT License](LICENSE) 开源。用户另行提供的参考图片、视频和品牌素材仍遵循其原有权利与许可。
