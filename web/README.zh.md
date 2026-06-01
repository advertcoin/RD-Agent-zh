# R&D-Agent

## 项目设置

```
npm install
```

### 开发环境编译和热重载

```
npm run dev
```

### 生产环境编译和压缩

```
npm run build
```

### 构建后的 API URL 行为

本项目使用当前页面的 origin 作为 API 基础 URL。

如果构建后的前端由同样暴露 `/upload`、`/trace`、`/control` 和其他 API 的 Flask 服务器提供服务，则不需要额外的前端配置。前端会自动调用提供页面的相同主机和端口。

此模板应帮助您开始使用 Vue 3 和 TypeScript 在 Vite 中进行开发。该模板使用 Vue 3 `<script setup>` SFC，查看 [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) 了解更多信息。

## 推荐设置

- [VS Code](https://code.visualstudio.com/) + [Vue - Official](https://marketplace.visualstudio.com/items?itemName=Vue.volar)（以前称为 Volar）并禁用 Vetur

- 使用 [vue-tsc](https://github.com/vuejs/language-tools/tree/master/packages/tsc) 从命令行执行相同的类型检查，或为 SFC 生成 d.ts 文件。