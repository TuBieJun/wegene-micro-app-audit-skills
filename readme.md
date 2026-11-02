# WeGene 微解读代码审核 Skill

用于辅助审核微基因微解读 ZIP 代码包中的**明显代码错误、文献参数不一致及 no_call 处理**。由支持 Agent Skills 的编码助手读取执行，不是独立运行程序。

## 审核范围

- effect allele 与 beta/OR 是否按原始论文及附件正确配对，包括效应方向和分层。
- 缺失键、`--`、`__` 是否进入正确处理路径；基因型剂量是否支持 0/1/2。
- 频率补偿、beta 累加、OR 累乘等是否存在明确计算错误。
- WeGene 大写 `RS...` 输入键是否被正确读取。

**不审核：**方法严谨性、少量离散评分的正态百分位、模型校准/外部验证、人格或科普文案、stdout/stderr、异常提示友好性、收费与部署规范。缺失采用跳过、补偿或停止本身不决定通过与否，检查的是明确错误及策略实现。

## 安装与使用

仓库：https://github.com/TuBieJun/wegene-micro-app-audit

本仓库采用单技能布局，根目录即技能目录，`SKILL.md` 为入口。

### 全局安装（Zed）

```sh
mkdir -p ~/.agents/skills
git clone https://github.com/TuBieJun/wegene-micro-app-audit.git ~/.agents/skills/wegene-micro-app-audit
```

### 项目级安装

在需要使用技能的项目根目录运行：

```sh
mkdir -p .agents/skills
git clone https://github.com/TuBieJun/wegene-micro-app-audit.git .agents/skills/wegene-micro-app-audit
```

两种方式任选其一；目标目录已存在时不要覆盖。安装后在 Zed 新对话中调用技能。若不使用 Git，也可将根目录的 `SKILL.md`、`report-template.md`、`references.md` 一起复制到上述 `wegene-micro-app-audit` 目录中。

安装目录名应为 `wegene-micro-app-audit`，与技能 frontmatter 的 `name` 一致。仅将本仓库打开为普通项目，并不意味着根目录的技能会被自动发现；应安装到客户端支持的技能目录。

通过 Git 安装的副本可使用以下命令更新（项目级安装时替换路径）：

```sh
git -C ~/.agents/skills/wegene-micro-app-audit pull --ff-only
```

启动示例：

> 使用 wegene-micro-app-audit 审核微解读应用。

助手会通过文字交互提示选择“开始新审核”或“继续已有审核”，并询问：

```text
代码 ZIP 路径：/实际路径/app.zip
文献标题 / 链接 / DOI：对应论文信息
```

也可以一次提供完整材料：

> 使用 wegene-micro-app-audit 审核 /实际路径/app.zip，文献为 https://论文地址。

已提供完整材料时直接开始；只缺一项时仅询问缺失项。联网读取文献可能需要授权；附件无法获得时会明确报告证据不足。

## 结果

按 no_call/剂量、文献参数、明显计算错误、rsid 读取四项报告：通过、不通过、证据不足或不适用。

- 存在明确错误：用户应用未通过审核。
- 无明确错误但关键证据不足：审核未完成，暂不能判定通过。
- 全部适用项通过：用户应用通过审核。

通过只限于上述审核范围，不代表平台批准、模型科学有效性或临床验证。

## 安全与隐私

默认静态只读审查，不运行/导入提交代码，不安装其依赖，不上传基因型或 ZIP。压缩包经过路径和资源检查后读取。原始测试材料位于 `test_material/`，已被 Git 忽略；不要提交真实遗传数据、私人代码包、凭据或含敏感信息的审核报告。

## 项目结构

```text
wegene-micro-app-audit/
├── SKILL.md             # 技能入口、启动交互和裁决规则
├── report-template.md   # 审核报告模板
├── references.md        # 来源、复算及审核边界回归场景
├── prompt.md            # 需求记录（安装时非必需）
├── readme.md            # 安装与使用说明
└── .gitignore           # 排除本地测试材料
```

## 维护与版本管理

通过 Git 维护技能及文档，提交前同步检查三个技能文件的审核边界，并按 `references.md` 的场景评估变化。文档或公式静态检查不等于完成真实应用的端到端审核。不要将排除范围内的问题重新引入报告模板或建议。
