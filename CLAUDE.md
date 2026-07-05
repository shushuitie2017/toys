# 玩具工坊 · Toys（toys）— 项目说明

浏览器里的**木质积木玩具工坊** web app：拼零件（方块/木板/轮子/球）、设转轴，切「玩耍」模式后真实物理让玩具动起来，可录短片 + URL 分享。**纯静态站，无需构建**（依赖走 CDN importmap）。

## 运行

```bash
# 纯静态，任意静态服务器起在项目根即可（ES module + importmap 需 http，不能 file://）
python -m http.server 5031        # http://127.0.0.1:5031/
```

⚠️ 需 **WebGPU 浏览器**（Chrome/Edge 113+）；用 Three.js `WebGPURenderer`，无 WebGL 回退。

## 结构

```
index.html      UI 骨架 + 内联 CSS + importmap（CDN 依赖）
main.js         全部逻辑：TYPES 零件定义、调色板、拖拽拼接、物理、玩耍、录制、分享
mp4.js          MP4 muxer（视频导出，无 UI）
docs/preview-cn.png  README 用截图
```

依赖（CDN importmap，勿本地化除非离线需求）：
- `three` / `three/webgpu` / `three/tsl` / `three/addons/` → jsdelivr 的 mrdoob/three.js@dev
- `box3d.js/inline` → jsdelivr npm box3d.js

## 中文化范围（已完成）

- UI 全中文：模式（搭建/玩耍）、工具栏（旋转：关·⟳·⟲ / 复制 / 删除）、分享（分享/下载视频/分享链接/复制/关闭）、零件名（方块/木板/轮子/球）、玩具列表（全部玩具/玩具 N·数/+ 新玩具）、所有提示文案、录制/加载文案。
- README、index.html title/meta 中文化。
- `main.js` 里 `console.warn` 调试信息保留英文（开发者可见，非 UI）；CSS 类名/元素 ID/body class（build/play）是内部标识，未动。

## 部署（GitHub Pages，已配 workflow）

- `.github/workflows/deploy.yml`：**无构建**，直接 `upload-pages-artifact path: .` 上传根目录 + `deploy-pages`。
- 线上：https://shushuitie2017.github.io/toys/
- 首次需在仓库 Settings → Pages → Source 选 **GitHub Actions**（一次性）。之后每次 push main 自动部署。

## 改动注意

- 改零件：`main.js` 顶部 `TYPES` 对象（`name` 是中文显示名，`kind`/mount/friction 等是逻辑，勿动 key）。
- 改 UI 文案：直接改 `main.js` 里对应 `textContent`/字符串字面量（已全中文，保持一致）。
- 加零件图标：`TYPES[x].icon` 是内联 SVG 路径。
