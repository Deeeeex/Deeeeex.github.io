# Jinhao Chen — academic homepage

个人学术主页与学习笔记，共用 GitHub Pages 站点 <https://deeeeex.github.io/>。

## 页面与发布结构

| 路径 | 内容 |
| --- | --- |
| `/` · `index.html` | 学术主页：研究方向、论文、开源项目和联系方式。 |
| `/blog/` · `blog/index.html` | 原有 Jekyll 博客首页。 |
| `/blog/page2/` 等 | 博客分页，由 `_config.yml` 中的 `paginate_path: /blog/page:num/` 生成。 |
| `_posts/`、`_layouts/`、`_includes/` 及原有资源目录 | 保留博客文章、主题和资源；既有文章与资源 URL 继续沿用。 |

仓库沿用现有 GitHub Pages 配置，从 `master` 分支发布。推送后可在仓库的 **Actions** 和 **Settings → Pages** 查看构建与发布状态；本文不代表某次部署已经完成。

学术主页为独立 HTML，样式、SVG 图示和脚本均内嵌，不需要额外的前端构建。博客仍由 Jekyll 生成。**不要给根目录 `index.html` 添加 YAML front matter**：其中 BibTeX 含有连续花括号，应作为原始 HTML 复制，避免被 Liquid 模板解析。

## 编辑主页

- 搜索 `id="name"`：姓名、机构、简介与主页链接。
- 搜索 `id="research"`：研究方向。
- 搜索 `class="pub"`：论文、作者、发表状态、链接与 BibTeX。
- 搜索 `id="code"`：开源项目。
- 搜索 `id="contact"`：邮箱及个人资料链接；Blog 指向 `/blog/`。
- 页面顶部 `:root`：颜色与字体。

主体内容和 BibTeX 折叠面板不依赖 JavaScript；脚本只增强复制和打印。右上角 SVG 是研究概念示意图，不是实验结果。页面包含窄屏、键盘焦点、减少动态效果及打印样式。

## 信息依据

主页随附资料的核对日期为 **2026-10-30**。信息来源如下；后续更新时应重新核对发表状态和链接，arXiv 文章按 preprint 标注。

- GitHub：<https://github.com/Deeeeex>
- Google Scholar：<https://scholar.google.com/citations?user=DRzNu6oAAAAJ>
- 原个人博客与公开邮箱来源：<https://deeeeex.github.io/>（博客入口现为 `/blog/`）
- Grounding with Confidence：<https://arxiv.org/abs/2609.39883>
- MetaVideoAgent：<https://arxiv.org/abs/2608.04587>
- MathSE 官方论文：<https://ojs.aaai.org/index.php/AAAI/article/view/39100>
- MathSE 项目：<https://zheny2751-dotcom.github.io/MathSE.github.io/>
- MathGLM-Vision：<https://arxiv.org/abs/2409.13729>
- Grounding 代码：<https://github.com/Alibaba-VELLDEPTH/Grounding-with-Confidence>
- MetaVideoAgent 代码：<https://github.com/Alibaba-VELLDEPTH/MetaVideoAgent>
- SmartAnalysis：<https://github.com/Deeeeex/SmartAnalysis>

## 博客主题致谢与许可证

1. 原博客模板 fork 自 [BY](https://github.com/qiubaiying/qiubaiying.github.io)，感谢作者 BY。
2. 同时感谢其上游模板 [Hux](https://github.com/Huxpro/huxpro.github.io)。
3. 感谢 Jekyll、GitHub Pages 和 Bootstrap。

保留仓库原有 MIT 许可证及版权声明，详见 [LICENSE](LICENSE)。
