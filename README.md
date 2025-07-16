# 📦 Hugo 广告模块（hugo-ad-modules）中文说明文档

该模块旨在为 Hugo 多站群系统提供一个高度模块化、灵活可配置的广告插入系统，支持多种广告类型、展示位置与控制逻辑，确保结构解耦、内容可控，并可与任何主题或站点配合使用。

---

## ✅ 一、模块用途

* 提供统一的广告插槽模板与渲染逻辑（slot-\*.html）
* 支持 4 种广告渲染模式：`demo` / `image` / `adsense` / `custom`
* 支持灵活的广告位启用/禁用控制
* 所有广告内容、位置、样式均可配置、可继承
* 支持多站复用、完全解耦站点内容

---

## 🧱 二、模块结构说明

```text
layouts/
├── partials/
│   └── ads/
│       ├── render.html                 # 广告渲染分发器
│       ├── google-adsense.html        # AdSense 渲染模板
│       ├── slot-ad-top.html           # 文章页顶部广告
│       ├── slot-ad-bottom.html        # 文章页底部广告
│       ├── slot-ad-inline.html        # 正文中部广告
│       ├── slot-ad-tags.html          # 标签页中部广告
│       ├── slot-ad-tags-top.html      # 标签页顶部广告
│       ├── slot-ad-list-inline.html   # 列表页中部广告
│       ├── slot-ad-home-inline.html   # 首页文章列表顶部广告
│       └── custom/                    # 自定义广告插槽目录
│           ├── ad-top.html
│           ├── ad-bottom.html
│           └── ...
static/
└── ads/
    ├── top.webp
    ├── bottom.webp
    └── ...
```

---

## ⚙️ 三、配置说明（config.yaml）

```yaml
params:
  ads:
    mode: image  # demo / image / adsense / custom

    adTop: true
    adBottom: true
    adInline: true
    adTags: true
    adTagsTop: true
    adListInterval: true

    imageMap:
      ad-top:
        img: "/ads/top.webp"
        link: "https://example.com/top"
        alt: "顶部广告"
      ad-bottom:
        img: "/ads/bottom.webp"
        link: "https://example.com/bottom"
      ad-inline:
        img: "/ads/inline.webp"
        link: "https://example.com/inline"
      ad-home-inline:
        img: "/ads/home.webp"
        link: "https://example.com/home"

    adsense:
      client: "ca-pub-xxxxxxxxxxxxxxxx"
      slots:
        ad-top: "1234567890"
        ad-bottom: "2345678901"
        ad-inline: "3456789012"
```

---

## 🧠 四、广告渲染流程

1. 页面模板中插入：

   ```gohtml
   {{ partial "ads/render.html" (dict "slot" "ad-top" "context" .) }}
   ```
2. `render.html` 判断当前 slot 是否启用
3. 根据 `mode` 渲染相应模板：

   * `demo`：占位图
   * `image`：读取 `.Params.ads.imageMap`
   * `adsense`：渲染 `google-adsense.html`
   * `custom`：渲染 `ads/custom/ad-xxx.html`

---

## 🧩 五、模式说明

| 模式      | 说明                                                  |
| ------- | --------------------------------------------------- |
| demo    | 展示默认占位图，用于调试或无广告时占位                                 |
| image   | 通过 `imageMap` 自定义图片广告，适用于静态图 + 链接形式                 |
| adsense | 使用 Google AdSense 动态广告，通过 `adsense.client/slots` 控制 |
| custom  | 渲染自定义 partial，适用于复杂逻辑、脚本或第三方联盟广告                    |

---

## 🛠 六、自定义广告模板（custom 模式）

> 所有模板放置于 `layouts/partials/ads/custom/` 下，文件名需为 `ad-[slot].html`

```html
<!-- ad-inline.html -->
<div class="ad-slot ad-inline">
  <!-- 任意 HTML、JS 广告逻辑 -->
  <script>console.log("加载了自定义广告");</script>
</div>
```

---

## 🧼 七、最佳实践

* 所有广告位使用统一的 `slot-ad-xxx.html` 结构，避免代码重复
* 图片广告资源统一放置 `static/ads/`
* 所有广告配置均通过 `config.yaml` 注入，不写死在模板内
* `render.html` 实现集中调度，可扩展性强
* 对于大型项目，可增加 `ads/debug.html` 或日志辅助诊断

---

## ✅ 八、结语

本模块设计为 **100% 可复用、解耦、易扩展** 的 Hugo 广告系统，适用于单站、多站、模板商品化等场景，欢迎按需扩展支持的广告位与样式结构。
