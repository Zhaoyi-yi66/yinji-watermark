# 印记 · 图片水印工作台

一个中文、免费、纯浏览器运行的图片水印小工具。MIT 开源，可商用和二次开发。

在线使用：https://yinji-watermark.sitemap-huoqu.workers.dev/

GitHub：https://github.com/Zhaoyi-yi66/yinji-watermark

## 功能
- 批量导入 JPG / PNG / WebP，拖放上传、缩略图切换
- 多行文字或 Logo 水印，单个水印 / 全图平铺
- 颜色、大小、透明度、旋转、间距、五个位置、阴影
- PNG / JPG / WebP 原分辨率导出，批量 ZIP 下载（无需第三方依赖）
- 移动端布局，本地生成示例图片，无图片上传、无统计脚本

## 本地使用
直接打开 `dist/index.html` 即可使用，无需安装依赖。也可运行 `python3 -m http.server 8080 --directory dist`。

单张输入限制 30 MB / 4000 万像素。批量处理受设备内存限制，大图建议分批处理。仅支持静态图片；Canvas 导出不保留原始 EXIF。极长水印可能超出图片，请缩小字号或换行。

## 发布到 GitHub
将本项目全部文件放入公开仓库，例如 `yinji-watermark`。请保留 LICENSE。

## Cloudflare Workers 部署
已提供 `wrangler.jsonc`，静态目录为 `dist`，无需构建。

方式一：Cloudflare 后台连接 GitHub 仓库，创建 Worker，构建命令留空，部署命令填写 `npx wrangler deploy`。

方式二：本地安装 Node.js 后运行：
```sh
npm install
npx wrangler login
npm run deploy
```
需要由账号持有人完成 Cloudflare 登录授权。实际域名以部署结果为准。

也可使用 Cloudflare Pages：选择 GitHub 仓库，框架预设 None，构建命令留空，输出目录 `dist`。

## 参考调研
- https://github.com/Alirewa/watermark-builder — 浏览器本地批量水印思路
- https://github.com/xyh9949/image-watermark — 文字、图片、平铺水印功能参考

本项目为独立实现，未复制上述项目源码，无外部运行时依赖。

## 验证
`node --check dist/app.js`：JavaScript 语法。
ZIP 输出已通过 Python zipfile 完整性检查；文字、平铺和 Logo 渲染路径已执行模拟检查。尚未完成真实浏览器交互及跨浏览器导出验证。

2026-10-19 发布检查：使用真实 Canvas 执行文字、平铺、Logo 及 PNG 导出路径，验证导出期间设置锁定；ZIP 通过 CRC、UTF-8 文件名和内容回读校验。增加了导出 MIME 类型校验，避免浏览器不支持格式时产生扩展名错误的文件。
