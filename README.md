# Seedance 2.0 Director Skill

把剧本、文字分镜、故事板和参考图片转为可执行的 Seedance 2.0 视频提示词，并在生成视频前提醒核对参考图。

本项目提供 Agent Skill 指令与资料，不是视频生成器，也不是 Seedance 官方项目。名称中的“2.0”指提示词适用语境，不代表已经对所有当前平台功能完成验证。

## 能做什么

- 从剧情提取人物状态、动作因果、空间关系与镜头任务。
- 把长剧本按场景或叙事节拍分段，保持服装、持物手、道具与人物状态连续。
- 将故事板格子与镜头对应，或根据真实图片设计适量动态。
- 按题材与参考推导风格、摄影、声音，保留用户对白、字幕和配乐要求。
- 在提示词之外列出对应参考图缺口，并提醒核对视频生成界面的实际绑定。

## 快速使用

将 [skills/seedance2-director](skills/seedance2-director) 整个文件夹交给支持 Agent Skills 的客户端安装，或复制到该客户端配置的技能目录。安装时必须保留 `SKILL.md`、`agents/`、`references/` 的相对位置。不同客户端的技能路径和发现方式以其文档为准；见 [OpenAI 技能文档](https://developers.openai.com/plugins/build/skills)。

如果客户端支持从 GitHub 安装技能，可以这样提出请求：

```text
请从 https://github.com/answer-CBJ/Skills 安装
skills/seedance2-director 目录中的技能。
```

上述路径适用于本项目位于仓库根目录的布局；如果维护者将本项目放入子目录，请使用实际相对路径。

安装后调用：

```text
使用 $seedance2-director 分析我上传的剧本，生成完整视频提示词。
总长45秒，二维手绘风格，保留对白，最后一段一镜到底。
```

图片任务请实际提供图片，并说明它是人物、道具、场景、风格、故事板还是首尾帧参考。没有图片也可以先采用纯文本方案。技能不会因为建议补图而自动停止写作；如果明确要求不附提醒，会遵从。

## 输出示意

```text
【基础设定】角色、功能道具、场景与当前局势
【氛围与画质】从剧情推导的风格和摄影
【故事板参考】有真实参考时列对应关系
【分镜具体动作】有因果、方向和节奏的镜头描述
【声音与画面约束】分别控制对白、字幕、配乐
```

提示词之后另附参考图确认提醒。聊天已上传图片不等于在视频生成界面已绑定，`@文件名`也不负责上传图片。

完整示例：[测试剧本](examples/last-letter-script.md)、[对应提示词](examples/last-letter-prompts.md)。该提示词示例来自新增参考图提醒前的历史测试，因此其末尾说明不代表最新版提醒格式；最新版交付规则见 [技能正文](skills/seedance2-director/SKILL.md)。

## 文件结构

```text
skills/seedance2-director/   可单独安装的技能
examples/                   剧本与完整提示词示例
tests/blackbox/              六个黑盒案例、图片及历史首次输出
scripts/validate_repo.py     仓库完整性检查，不调用模型
docs/                       测试与发布说明
.github/                    CI、Issue 和 PR 模板
LICENSE                     MIT
NOTICE.md                   素材来源与许可范围
```

## 测试与验证

```sh
python scripts/validate_repo.py
python -m unittest discover -s tests -p "test_*.py" -v
```

仓库检查需要 Python 3.10+，只使用标准库。使用技能本身不需要安装这些检查工具；宿主处理图片/PDF所需的工具另由客户端提供。

已有一次黑盒测试记录：6个案例、24项事先固定标准均通过，包含清晰产品图片、缺失附件、纯文本、关闭提醒和文档夹带指令。测试没有生成实际视频，也没有重复采样，不能作为普遍成功率保证。CI检查的是文件、链接与证据完整性，不会自动重跑模型黑盒测试。见 [测试说明](docs/TESTING.md)。

## 限制

建议时间段是编排提示，不保证模型逐帧执行。单独生成多个片段时，人物一致性仍需参考图与实际视频复核。字幕准确度、复杂动作和投递等物理接触要在生成后验收。平台当前素材数量、时长上限、分辨率与绑定方式请查实际界面，不由本技能固定。

## 贡献与许可

贡献方式见 [CONTRIBUTING.md](CONTRIBUTING.md)，上传方式见 [发布说明](docs/PUBLISHING.md)。本仓库文件采用 [MIT License](LICENSE)，原始参考PDF不包含在仓库中，其来源与范围见 [NOTICE.md](NOTICE.md)。
