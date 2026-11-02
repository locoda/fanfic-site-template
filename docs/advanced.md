# 进阶教程

网站已经按 [README](../README.md) 建好、能正常写文了，再来看这份。前面几节都在网页上点点就能做完，不用写代码；最后的「改代码」才需要一点技术，或者交给 AI。

**目录**

- [改用 Cloudflare Pages 部署](#改用-cloudflare-pages-部署)
- [绑定自己的域名](#绑定自己的域名)
- [更多功能怎么用](#更多功能怎么用)
- [请朋友一起管理](#请朋友一起管理)
- [备份全部作品](#备份全部作品)
- [跟上模板的更新](#跟上模板的更新)
- [改代码：二次开发入门](#改代码二次开发入门)

## 改用 Cloudflare Pages 部署

默认的 GitHub Pages 已经够用。下面几种情况可以考虑换成 Cloudflare Pages（免费）：

- **不想公开原始文件**：免费的 GitHub Pages 要求仓库是公开的，Cloudflare 可以用私有仓库
- **国内访问**：`github.io` 在国内时好时坏，换 Cloudflare 有时会稳一些（也不保证，最稳的是再绑一个自己的域名）
- **想先预览再上线**：Cloudflare 会给每个分支生成一个预览网址，改动合并之前就能看效果

步骤：

1. 注册一个 [Cloudflare 账号](https://dash.cloudflare.com/sign-up)。
2. 在网站后台 **网站设置** → **网站信息** 里，关掉 **部署到 GitHub Pages**，保存。不关的话两边都会部署，也没坏处，只是多一份。
3. 登录 [Cloudflare](https://dash.cloudflare.com)，左边选 **Workers & Pages** → **Create application** → **Pages** 标签 → **Import an existing Git repository**。
4. 连接 GitHub，选中你的网站仓库，点 **Begin setup**。
5. 填三样：**Production branch** 填 `main`，**Build command** 填 `npm run build`，**Build output directory** 填 `dist`。**Project name** 会变成网址的一部分：`项目名.pages.dev`。
6. 点 **Save and Deploy**，等两三分钟。

之后每次在后台保存，Cloudflare 都会自动重新部署。部署失败的话，到 Cloudflare 项目的 **Deployments** 页面点进那一次看原因。

**想把仓库改成私有**：换好 Cloudflare 以后，到 GitHub 仓库 **Settings** → 最下面 **Danger Zone** → **Change repository visibility** → **Make private**。Pages CMS 后台照常能用。

## 绑定自己的域名

先要有一个域名（在任意域名服务商买，比如 Cloudflare、Namecheap、阿里云）。可以用整个域名（`example.com`），也可以用它下面的一个子域名（`fanfic.example.com`）。子域名设置起来更简单，推荐。

### 用 GitHub Pages 的

1. 到仓库 **Settings** → **Pages** → **Custom domain**，填上你的域名，点 **Save**。
2. 到你买域名的地方，找到「DNS 设置」或「解析设置」，加记录：
   - **子域名**（比如 `fanfic.example.com`）：加一条 **CNAME** 记录，主机名填 `fanfic`，值填 `你的GitHub用户名.github.io`
   - **整个域名**（`example.com`）：加四条 **A** 记录，主机名填 `@`，值分别是 `185.199.108.153`、`185.199.109.153`、`185.199.110.153`、`185.199.111.153`
3. 等 DNS 生效（几分钟到几小时），回到 **Settings** → **Pages**，等它显示检查通过后，勾上 **Enforce HTTPS**。

网站的网址会自动换成新域名，不用改任何设置。

### 用 Cloudflare Pages 的

1. 在 Cloudflare 打开你的 Pages 项目 → **Custom domains** → **Set up a custom domain**，填上域名。
2. 如果这个域名本来就在 Cloudflare 管理，它会自动加好记录，点确认就行。
3. 如果域名在别处管理：
   - **子域名**：按页面提示，到域名服务商那里加一条 **CNAME**，主机名填子域名那部分（比如 `fanfic`），值填 `项目名.pages.dev`
   - **整个域名**：Cloudflare 要求把域名的 DNS 交给它管理（页面上会引导你改域名的 nameserver）。嫌麻烦就用子域名
4. 等状态变成 **Active**，就能用新域名打开了。原来的 `项目名.pages.dev` 也还能访问。

## 更多功能怎么用

下面这些都在后台 **网站设置** 里，改完点保存，一两分钟后生效。

- **换中文字体**（显示设置）：几款都适合长篇阅读，可以换着看效果。选「系统字体」最省流量，但不同电脑上看起来不一样。
- **首页头条**（显示设置）：打开「首页头条」后，首页第一篇会做成大标题。「头条用哪篇」默认是最新一篇；想一直推荐某一篇，选「指定一篇」，再在「指定的头条作品」里挑出来。改了那篇的作品名的话，要回来重新选。
- **禁止复制**（作品设置）：关掉「允许复制正文」，读者就选不中文字、不能复制和右键。挡得住随手搬运，挡不住会看网页源代码的人和截图。
- **多人合集**（作品设置）：打开「每篇作品单独写作者名」，每篇作品里就能填自己的作者名，没填的照旧用网站的作者名。适合合志、合集、几个人一起用的站。
- **系列**：几篇作品的「系列」写成一模一样的名字，就会自动串成一个系列，读完一篇能直接点下一篇。「系列里的第几篇」决定顺序。
- **一篇多个配对**：作品的「CP」里点「Add an item」多加几个，这篇会同时出现在每个配对的页面里。
- **批量导入时统一填信息**：「批量导入」页面点「开始导入」时，弹框里填的配对、系列、连载状态、日期，会用在这一批的每一篇上。填了系列的，按文件名顺序接在系列后面。

还有一些是自动的：没有任何作品写配对或系列时，导航里的「配对」「系列」会自动隐藏；作品多了首页会自动分页。

## 请朋友一起管理

在 Pages CMS 里可以用邮箱邀请协作者，对方不需要 GitHub 账号，登录后台就能写作品、改设置。具体在 Pages CMS 里打开你的仓库，找设置里的协作者（Collaborators）。

## 备份全部作品

1. 到 GitHub 仓库的 **Actions** 页面，左边选「导出作品」。
2. 点右边的 **Run workflow**。「发布方式」默认是草稿，只有你自己能看到；选「公开」的话，所有能看到仓库的人都能下载。
3. 几分钟后，到仓库的 **Releases** 页面下载 zip。里面每篇作品一个 Markdown 文件，任何文本编辑器都能打开，也能用「批量导入」再导回来。

## 跟上模板的更新

模板以后加了新功能、修了问题，你的网站不会自动跟着变，要「同步」一次。目前需要用到命令行：

```
git remote add template https://github.com/locoda/fanfic-site-template.git   # 第一次
git config merge.ours.driver true                                             # 第一次
git fetch template
git merge template/main --allow-unrelated-histories   # 第一次加这个参数，以后直接 git merge template/main
```

不会用命令行的话，可以把这一节交给 AI（见下面「让 AI 帮你改」），跟它说「帮我把 fanfic-site-template 的更新同步到我的网站，按 AGENTS.md 里的步骤做」。

几点注意：

- 合并进来的示例作品（文件名是 `example-` 开头的）删掉
- 你的 `src/site.yml`（网站设置）和 README 要保留你自己的版本。在仓库里加一个 `.gitattributes` 文件，写上下面两行，以后同步时会自动保留：
  ```
  README.md merge=ours
  src/site.yml merge=ours
  ```
- 模板新加了设置项的话，要把它补进你的 `site.yml`（不补也能用默认值，但后台的勾选框可能显示不对）
- 同步要开 PR 的话，合并时选 **Create a merge commit**，不要选 squash，不然下次同步会冲突一大堆

## 改代码：二次开发入门

想改样式、加功能，先想一想：

- **改了代码，以后同步模板时可能会冲突**。改得越多，同步越麻烦
- 如果是大家都用得上的改进（修 bug、加一个开关），欢迎直接提到[模板仓库](https://github.com/locoda/fanfic-site-template)，大家一起用，你也不用自己维护

### 让 AI 帮你改

不会写代码也能改。用能直接操作 GitHub 仓库的 AI 编程助手（比如 Claude Code），步骤大概是：

1. 把 AI 连上你的网站仓库。
2. 告诉它想改什么，并说一句「先读仓库里的 AGENTS.md」。这份文件写了这个项目的结构和规矩（网址不能变、不能用国外字体 CDN 之类），AI 照着做不容易改坏。
3. 让它开一个 PR，不要直接改 main。
4. 打开 PR 页面，看下面的**自动检查**：绿勾就是网站能正常生成；红叉点 **Details** 能看到哪里出了问题，可以把内容贴回给 AI 让它修。
5. 用 Cloudflare 的话，PR 页面还会有一个预览网址，点开看看效果。
6. 没问题就合并。

### 自己在电脑上改

需要装 [Node.js](https://nodejs.org)（22.23.4 以上）和 [git](https://git-scm.com)。

```
git clone https://github.com/你的用户名/仓库名.git
cd 仓库名
npm install
npm run dev        # 打开 http://localhost:4321 ，改了文件会自动刷新
```

常改的地方：

| 想改什么 | 改哪个文件 |
|---|---|
| 颜色、字号、间距 | `src/styles/global.css` 开头的变量 |
| 界面上的文字（「全部作品」「配对」等） | `src/site.config.ts` 里的 `ui` |
| 可选的中文字体 | `src/lib/fonts.mjs`（步骤见 [development.md](development.md)） |
| 页面结构 | `src/pages/` 和 `src/layouts/Base.astro` |

改完运行检查，通过了再提交：

```
npm run check      # 自动检查，和开 PR 时 GitHub 上跑的是同一套
git add -A
git commit -m "说明你改了什么"
git push
```

更详细的说明（项目结构、加开关的完整步骤、改完要检查什么）见 [技术要点](architecture.md) 和 [二次开发指南](development.md)。
