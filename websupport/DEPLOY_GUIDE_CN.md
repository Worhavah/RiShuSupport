# GitHub Pages 部署中文教程

本教程将指导你如何将 `websupport` 文件夹下的内容（技术支持页和隐私政策页）免费托管到 GitHub Pages 上，生成一个公开的网址，用于填写 App Store 的技术支持 URL。

---

## 准备工作
1. 确保你已经有一个 GitHub 账号。
2. 确保本项目已经上传到了 GitHub 仓库。

---

## 方案一：最简单方式 (直接使用当前分支)

如果你的仓库是公开的，或者你有 GitHub Pro，可以直接发布当前代码。

### 步骤：
1. **提交代码**：确保 `websupport` 文件夹及里面的 `index.html` 和 `privacy.html` 已经 push 到了 GitHub 仓库的 `main` (或 `master`) 分支。
2. **打开设置**：在 GitHub 仓库页面，点击顶部的 **Settings (设置)**。
3. **找到 Pages**：在左侧侧边栏中找到 **Pages** 选项（通常在 "Code and automation" 区域）。
4. **配置源 (Source)**：
   - 在 "Build and deployment" 下，将 **Source** 选为 **Deploy from a branch**。
   - 在 **Branch** 下拉菜单中选择 `main` (或你的主分支名)，文件夹选择 `/(root)`。
   - 点击 **Save (保存)**。
5. **获取链接**：
   - 等待几分钟（可以刷新页面），顶部会显示 "Your site is live at..."。
   - 你的技术支持页链接将是：
     `https://<你的用户名>.github.io/<仓库名>/rishu_node1_tx/websupport/index.html`
   - 你的隐私政策链接将是：
     `https://<你的用户名>.github.io/<仓库名>/rishu_node1_tx/websupport/privacy.html`

*缺点：链接比较长，且包含项目目录结构。*

---

## 方案二：推荐方式 (使用独立分支，链接更短)

这个方法会自动把 `websupport` 文件夹的内容发布到一个干净的 `gh-pages` 分支，这样你的网址会更短、更专业。

### 步骤：

#### 1. 创建自动部署流程
在项目根目录下（不是 websupport 目录，而是整个项目的最外层），创建以下目录和文件：
`.github/workflows/deploy-support.yml`

**文件内容如下：**

```yaml
name: Deploy Support Page

on:
  # 当推送到 main 分支，且修改了 websupport 文件夹下的内容时触发
  push:
    branches:
      - main
    paths:
      - 'rishu_node1_tx/websupport/**'
  # 允许手动触发
  workflow_dispatch:

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          # 这里指定要发布的文件夹路径
          publish_dir: ./rishu_node1_tx/websupport
```

#### 2. 推送配置
将这个新文件 commit 并 push 到 GitHub。

#### 3. 配置 GitHub Pages
1. 推送成功后，GitHub Actions 会自动运行（可以在仓库的 **Actions** 标签页查看进度）。等待它变成绿色勾勾 ✅。
2. 回到仓库的 **Settings** -> **Pages**。
3. 在 **Branch** 选项中，这次选择 **`gh-pages`** 分支（这是 Action 自动创建的）。
4. 文件夹选择 `/(root)`。
5. 点击 **Save**。

#### 4. 获取链接
等待一两分钟刷新，你的链接将会是：
- 支持页：`https://<你的用户名>.github.io/<仓库名>/`
- 隐私页：`https://<你的用户名>.github.io/<仓库名>/privacy.html`

---

## 常见问题 (FAQ)

**Q: 页面打开是 404 错误？**
A: 
1. 检查 URL 是否拼写正确，注意区分大小写。
2. 如果是方案一，确保 URL 包含了完整的路径 `/rishu_node1_tx/websupport/index.html`。
3. 如果是方案二，确保 GitHub Actions 已经成功运行，并且 Settings 里 Pages 的分支选对了 `gh-pages`。

**Q: 更新了页面内容，但网页没变？**
A: 浏览器有缓存。尝试强制刷新（Ctrl+F5 或 Cmd+Shift+R），或者等待几分钟，GitHub Pages 更新需要一点时间。

**Q: 我的仓库是私有的 (Private)？**
A: 私有仓库使用 GitHub Pages 功能需要 GitHub Pro 账户。如果你是免费账户，建议创建一个专门的公开 (Public) 仓库只用来存放这些支持页面文件。
