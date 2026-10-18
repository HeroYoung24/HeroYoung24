# 贪吃蛇贡献动画

主页的 SVG 由 `.github/workflows/snake.yml` 生成并发布到 `output` 分支。
动画读取 GitHub 贡献日历，不直接统计本地的 `git commit` 次数。

## 显示私有仓库贡献

默认的 `GITHUB_TOKEN` 不等于你的个人账号授权。你登录后看到的新绿格，
可能不在这个仓库令牌读取到的贡献日历中。

1. 在你自己的 GitHub 账号中打开
   [Personal access tokens (classic)](https://github.com/settings/tokens)，
   创建一个有有效期的令牌，并勾选 `read:user`。
   GitHub 的 `ContributionsCollection` 接口用这个权限读取私有贡献；
   这里不需要授予写仓库权限。
2. 打开本仓库的
   [Actions secrets](https://github.com/HeroYoung24/HeroYoung24/settings/secrets/actions)，
   新建 Repository secret，名称为 `SNAKE_GITHUB_TOKEN`，值为该令牌。
   不要把令牌写入代码、提交到仓库或粘贴到聊天中。
3. 将工作流修改推送到 `main`，在
   [generate animation](https://github.com/HeroYoung24/HeroYoung24/actions/workflows/snake.yml)
   页面点击 **Run workflow**，选择 `main` 运行。
4. 确认任务成功后，打开 `output` 分支的 SVG，检查近期有贡献的格子。
   主页若仍显示旧图，再刷新页面并排查图片缓存。

生成步骤优先使用 `SNAKE_GITHUB_TOKEN`；未配置时回退到 `GITHUB_TOKEN`，
并在日志和任务摘要中提示可能缺少私有贡献。
发布步骤仍使用仓库自带的 `GITHUB_TOKEN`。
令牌过期或撤销后，需要更新此 Secret。

**注意：生成的 SVG 放在公开的 `output` 分支，私有贡献的日期和活跃程度也会公开可见。**

## 更新频率与排查

- 向本仓库的 `main` 推送会触发生成；其他仓库的提交通过定时任务同步。
- 定时任务计划每两小时的第 17 分钟（UTC）运行，GitHub 调度可能延迟。
- 若任务成功但格子未变化，先比较登录后与未登录时的贡献图，并检查上述 Secret。
- 若贡献图本身缺少提交，再检查提交邮箱、默认分支和 GitHub 的统计延迟。
- 动画表现贡献日历的颜色等级，不显示准确的 commit 数字；同一天次数增加未必改变颜色。

参考：[GitHub 贡献接口权限](https://docs.github.com/en/graphql/reference/users#contributionscollection)、
[缺少贡献的排查](https://docs.github.com/zh/account-and-profile/how-tos/contribution-settings/troubleshooting-missing-contributions)、
[Platane/snk 参数](https://github.com/Platane/snk/blob/v3/svg-only/action.yml)。
