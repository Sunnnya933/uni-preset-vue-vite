# uni-preset-vue-vite

一个基于 Vue 3、Vite 和 uni-app 的移动端页面项目，当前包含首页、登录页、注册页以及对应的 HTML 原型页面。

## 在线地址

GitHub 仓库：<https://github.com/Sunnnya933/uni-preset-vue-vite>

GitHub Pages 部署完成后访问：<https://sunnnya933.github.io/uni-preset-vue-vite/>

## 技术栈

- Vue 3
- Vite 5
- uni-app
- GitHub Actions
- GitHub Pages

## 本地开发

安装依赖：

```bash
npm install
```

启动 H5 开发服务：

```bash
npm run dev:h5
```

构建 H5 产物：

```bash
npm run build:h5
```

## 页面结构

```text
src/pages/index/index.vue      首页
src/pages/login/index.vue      登录页
src/pages/register/index.vue   注册页
```

## Git 自动推送

本项目已配置本地 `post-commit` 钩子。后续在 `main` 分支执行提交后，会自动运行：

```bash
git push origin main
```

也可以手动推送：

```bash
git push
```

## GitHub Pages 部署

项目已添加 GitHub Actions 工作流。每次推送到 `main` 分支后，会自动构建并部署到 GitHub Pages。

如果首次访问 Pages 失败，请在 GitHub 仓库中检查：

1. 打开 `Settings`。
2. 进入 `Pages`。
3. `Build and deployment` 选择 `GitHub Actions`。
4. 等待 `Actions` 中的部署任务完成。
