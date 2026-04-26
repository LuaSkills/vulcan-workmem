# 仓库地址迁移与发布资料统一调整计划

## 任务目标

将当前仓库从旧有命名与地址表达统一调整为 `LuaSkills/demo-skill`，覆盖 README、帮助文档、资源说明、运行时展示内容、构建脚本、校验脚本以及发布包元数据，确保构建产物、文档说明和脚本行为保持一致。

## 详细执行步骤

1. 盘点仓库中与旧仓库名、旧技能目录名、发布包名称、GitHub 地址、安装说明相关的引用点。
2. 梳理 `skill.yaml`、`README.md`、`help/`、`resources/`、`overflow_templates/`、`scripts/`、`runtime/`、`licenses/` 等文件中需要同步的内容。
3. 将面向用户的仓库地址、安装示例、发布说明统一改为 `LuaSkills/demo-skill`。
4. 调整构建脚本与校验脚本，使发布包根目录、压缩包名称、校验规则和可选 `source.yaml` 输出与 `demo-skill` 保持一致。
5. 更新运行时与模板中展示的技能标识，避免仍输出旧技能标识。
6. 运行校验脚本和打包脚本，确认发布包、校验和、可选源元数据可以正常生成。
7. 对照本计划逐项核验，补齐遗漏引用，并记录执行变更总结。
8. 验证完成后，将本计划迁移到 `docs/completed/20260426/01-REPOSITORY_RENAME_ALIGNMENT.md`。

## 技术选型

- 使用现有 Python 脚本继续承担校验与打包职责，不引入新的构建工具或依赖。
- 使用结构化 YAML 读取现有 `skill.yaml` 中的版本信息，保持版本来源单一。
- 文档与脚本统一以 `demo-skill` 作为技能目录和发布包根目录，以 `LuaSkills/demo-skill` 作为 GitHub 仓库地址表达。
- 保持现有跨平台脚本入口，包括 PowerShell 与 Shell 标签脚本。

## 验收标准

1. 仓库内不再存在面向发布和安装流程的旧仓库名。
2. README、帮助文档、资源文档和许可证说明均指向 `LuaSkills/demo-skill` 或 `demo-skill`。
3. 打包脚本生成的 zip 文件名、包内顶层目录、`source.yaml` 元数据均使用 `demo-skill`。
4. 校验脚本认可当前仓库根目录名 `demo-skill`，并能通过严格结构校验。
5. `python .\scripts\validate_skill.py` 执行成功。
6. `python .\scripts\package_skill.py --emit-source-yaml --base-url https://github.com/LuaSkills/demo-skill/releases/download/v0.1.2` 执行成功，并生成预期产物。

## 执行变更总结

### 1. 核心修复与调整概述

已将仓库发布、安装、打包和运行时展示中涉及的仓库地址与技能标识统一调整为 `LuaSkills/demo-skill` 与 `demo-skill`，同步当前清单版本 `0.1.2`，并确认校验脚本与打包脚本均可正常运行。

### 2. 📂文件变更清单

- 新增：`docs/plan/20260426-01-REPOSITORY_RENAME_ALIGNMENT.md`，用于记录本次计划、验收标准和执行总结，闭环后迁移到 `docs/completed/20260426/01-REPOSITORY_RENAME_ALIGNMENT.md`。
- 修改：`README.md`，统一仓库标题、目录示例、发布版本示例、source metadata 说明和 release URL。
- 修改：`skill.yaml`、`help/help.md`、`resources/guide.md`、`licenses/THIRD_PARTY_NOTICES.md`，统一帮助与资源文档中的仓库地址表达。
- 修改：`runtime/demo_status.lua`、`overflow_templates/demo-page.md`，统一运行时返回的技能标识、版本号和模板展示内容。
- 修改：`scripts/package_skill.py`，将默认 `source.yaml` 发布资产 URL 调整为 `LuaSkills/demo-skill` 的 GitHub release 地址。
- 修改：`.github/workflows/release.yml`，将 release artifact 名称调整为 `demo-skill-release`。
- 删除：无。

### 3. 💻关键代码调整详情

- `normalize_base_url` 新增 manifest 版本参数参与默认 URL 构造，不传 `--base-url` 时会生成 `https://github.com/LuaSkills/demo-skill/releases/download/v{version}`。
- `build_source_metadata` 调用新的 URL 规范化逻辑，生成的 `source.yaml` 中 `source.locator`、`package.url`、`checksums.url` 均指向 `LuaSkills/demo-skill` 发布资产。
- `demo_status.lua` 将返回载荷中的 `skill_id` 调整为 `demo-skill`，并将 `skill_version` 同步为 `0.1.2`。

### 4. ⚠️遗留问题与注意事项

- 本次验证生成的 `dist/demo-skill-v0.1.2-*` 产物位于已忽略的 `dist/` 目录，仅作为本地验证结果保留。
- 仓库中已有未跟踪文件 `SKILL_DEVELOPER_MANUAL.md`，本次未修改该文件。
- 已执行 `python .\scripts\validate_skill.py`，结果通过。
- 已执行显式 base URL 与默认 base URL 两种 `python .\scripts\package_skill.py --emit-source-yaml` 打包路径，均成功生成 `demo-skill-v0.1.2-*` 产物。
