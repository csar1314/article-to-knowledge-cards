# 本地行为测试记录

按照当前 SKILL.md 手工执行四组输入，输出保存在同目录；逐张对照原文检查依据、单一知识点、去重及条件。未运行独立子代理或自动语义评测。

| 输入 | 输出 | 结果 |
| --- | --- | --- |
| science.md：科普 | science-cards.md，7 张 | 保留积水和饮用安全的限制，无额外事实 |
| procedure.txt：操作流程 | procedure-cards.md，6 张 | 保留停止使用和报告条件，以及无异常的日志要求 |
| opinion.md：观点文章 | opinion-cards.md，6 张 | 建议归于作者，没有将观察升级为因果 |
| short.txt：短文本 | short-cards.md，2 张 | 合并重复句，解释数量不足，不凑 5 张 |

复核后将短文本不足数量、观点归属和相关不等于因果明确写入 Skill。没有原文例子时使用有依据答案的自测问题。

这些示例可供复测，不代表对所有文章质量的保证。官方 quick_validate.py 结构验证通过（Skill is valid!）。验证环境的 PyYAML 安装在项目的 .validation-deps 中，不属于 Skill 运行依赖。
