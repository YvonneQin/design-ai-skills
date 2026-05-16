# Annotation Guide — Primary Action Annotation

每次 Construction 出图后**默认执行**。在触发跳转下一页面的主 CTA 按钮上添加原子级标注。

---

## 各页面的主 CTA 定义

| 页面 Skill | 搜索关键词（按优先级） | 目标页面 |
|-----------|----------------------|---------|
| login | `Sign In`、`Sign Up`、`Submit` | → Dashboard |
| dashboard | `Create`、`Get Started`、`View All` | → List Page |
| list-page | `Create`、`+ Create`、`New` | → Create Page |
| create-page | `Create`、`Submit`、`Order`、`Confirm` | → Order Detail |
| detail-page | `Edit`、`Manage`、`Delete` | — |
| order-detail | `Pay`、`Confirm`、`Complete` | — |

关键词匹配逻辑：Zenlayer 组件库将**所有** Button instance 命名为 `"Button"`（非语义名），关键词匹配对 name 无效。实际策略：**找 frame 下 y > frameTop+200 区域内 width 最大的 `INSTANCE[name="Button"]`**（即导航栏以下最宽的按钮 = 主 CTA）。

---

## 标注样式规范

| 元素 | 规格 |
|------|------|
| 高亮框 | 虚线描边 `dashPattern [5,4]`，`strokeWeight 1.5`，fill opacity 0.10，`cornerRadius 6`，padding 4px |
| Badge | 圆形 frame，`cornerRadius 99`，品牌黄背景，深色文字，字号 11 SemiBold |
| Callout tag | 深色 navy 背景（`#0d0f24`），品牌黄描边，正文 12 Regular，padding 9×6 |
| 连接线 | `dashPattern [3,3]`，opacity 0.5，`strokeWeight 1` |
| 颜色 | 品牌黄 `{r:1, g:0.78, b:0.1}` 用于主 CTA；品牌蓝 `{r:0.09, g:0.39, b:1}` 用于辅助元素 |

所有标注节点放入 `Annotations / {页面名}` group，不影响原 frame 结构。

---

## figma-use 代码模板

将 `FRAME_NODE_ID`、`PAGE_KEYWORDS`、`DESTINATION_LABEL` 替换为实际值后执行：

```javascript
await figma.loadFontAsync({ family: 'Inter', style: 'SemiBold' });
await figma.loadFontAsync({ family: 'Inter', style: 'Regular' });

// ── 配置 ─────────────────────────────────────────────────
const FRAME_ID        = '<FRAME_NODE_ID>';
const PAGE_KEYWORDS   = ['<keyword1>', '<keyword2>'];  // 按优先级
const DEST_LABEL      = '→ <目标页面>';

// ── 颜色 ─────────────────────────────────────────────────
const YELLOW = { r: 1.0,  g: 0.78, b: 0.1  };
const NAVY   = { r: 0.05, g: 0.06, b: 0.14 };
const ids    = [];

// ── 获取节点绝对坐标 ──────────────────────────────────────
function absPos(node) {
  let x = 0, y = 0, cur = node;
  while (cur && cur.type !== 'PAGE') {
    if ('x' in cur) { x += cur.x; y += cur.y; }
    cur = cur.parent;
  }
  return { x, y };
}

// ── 遍历找主 CTA（Zenlayer 库所有按钮名均为 "Button"，用宽度代替关键词） ──
// frameTop+200 排除导航栏区域，取最宽按钮 = 主 CTA
const page  = figma.currentPage;
const frame = await figma.getNodeByIdAsync(FRAME_ID);
if (!frame) return { error: 'frame not found' };

const frameTop = frame.y;
let cta = null, maxW = 0;
function findWidestBtn(node, depth) {
  if (depth > 8) return;
  if (node.type === 'INSTANCE' && node.name === 'Button' && 'width' in node) {
    const p = absPos(node);
    if (p.y > frameTop + 200 && node.width > maxW) {
      maxW = node.width;
      cta = node;
    }
  }
  if ('children' in node) {
    for (const c of node.children) findWidestBtn(c, depth + 1);
  }
}
findWidestBtn(frame, 0);
if (!cta) return { error: 'CTA not found — no Button instance below nav area' };

const pos = absPos(cta);
const { x, y, width: w, height: h } = { ...pos, width: cta.width, height: cta.height };
const PAD = 4;

// ── 高亮框 ────────────────────────────────────────────────
const hlBox = figma.createRectangle();
page.appendChild(hlBox);
hlBox.x = x - PAD; hlBox.y = y - PAD;
hlBox.resize(w + PAD * 2, h + PAD * 2);
hlBox.fills   = [{ type: 'SOLID', color: YELLOW, opacity: 0.10 }];
hlBox.strokes = [{ type: 'SOLID', color: YELLOW }];
hlBox.strokeWeight = 1.5;
hlBox.cornerRadius = 6;
hlBox.dashPattern  = [5, 4];
hlBox.name = 'annotation/highlight';
ids.push(hlBox.id);

// ── Badge ─────────────────────────────────────────────────
const bdg = figma.createFrame();
page.appendChild(bdg);
bdg.fills = [{ type: 'SOLID', color: YELLOW }];
bdg.cornerRadius = 99;
bdg.name = 'annotation/badge';
const bdgT = figma.createText();
bdg.appendChild(bdgT);
bdgT.characters = '▶';
bdgT.fontSize   = 10;
bdgT.fontName   = { family: 'Inter', style: 'SemiBold' };
bdgT.fills      = [{ type: 'SOLID', color: NAVY }];
bdgT.x = 5; bdgT.y = 4;
bdg.resize(bdgT.width + 10, bdgT.height + 8);
bdg.x = x + w - 8;
bdg.y = y - bdg.height / 2;
ids.push(bdg.id);

// ── Callout tag ───────────────────────────────────────────
const TAG_X = x + w + 12;
const TAG_Y = y + h / 2 - 14;
const tag = figma.createFrame();
page.appendChild(tag);
tag.fills   = [{ type: 'SOLID', color: NAVY }];
tag.strokes = [{ type: 'SOLID', color: YELLOW }];
tag.strokeWeight = 1.5;
tag.cornerRadius = 5;
tag.name = 'annotation/callout';
const tagT = figma.createText();
tag.appendChild(tagT);
tagT.characters = `${DEST_LABEL}`;
tagT.fontSize   = 12;
tagT.fontName   = { family: 'Inter', style: 'Regular' };
tagT.fills      = [{ type: 'SOLID', color: YELLOW }];
tagT.x = 9; tagT.y = 6;
tag.resize(tagT.width + 18, tagT.height + 12);
tag.x = TAG_X; tag.y = TAG_Y;
ids.push(tag.id);

// ── 连接线 ────────────────────────────────────────────────
const ln = figma.createLine();
page.appendChild(ln);
ln.x = x + w + 4; ln.y = y + h / 2;
ln.resize(TAG_X - (x + w + 4), 0);
ln.strokes      = [{ type: 'SOLID', color: YELLOW, opacity: 0.5 }];
ln.strokeWeight = 1;
ln.dashPattern  = [3, 3];
ln.name = 'annotation/connector';
ids.push(ln.id);

// ── 分组 ─────────────────────────────────────────────────
const annNodes = ids.map(id => figma.getNodeById(id)).filter(Boolean);
const grp = figma.group(annNodes, page);
grp.name = `Annotations / ${frame.name}`;

return { ctaName: cta.name, ctaPos: { x, y, w, h }, groupId: grp.id };
```

---

## Prototype 连线（原子触点）

在出图后如需添加可点击的 Prototype 连线，且触点需精确到按钮层级：

### 核心限制与方案

- `reactions` 属性是只读的 → 必须用 `setReactionsAsync()`
- `transition: null` 无效 → 必须提供完整 transition 对象
- **页面级裸矩形/Frame 不能作为 prototype 触发节点** → 必须是某个 screen Frame 的子节点
- Instance 的 children 是只读的 → 无法直接往 Instance 里插节点

### Wrapper + Hotspot 模式（已验证）

将 Instance 包入一个 wrapper Frame，再在 wrapper 内插透明 hotspot Frame：

```javascript
// 1. 找到 Instance 及按钮的相对坐标
const inst = await figma.getNodeByIdAsync('<instance_id>');
// relX/relY = absPos(ctaNode) - inst.x/inst.y

// 2. 创建 wrapper Screen（与 Instance 同尺寸、同位置）
const wrapper = figma.createFrame();
wrapper.name = "Screen / <页面名>";
wrapper.x = inst.x;  wrapper.y = inst.y;
wrapper.resize(inst.width, inst.height);
wrapper.fills = [];
wrapper.clipsContent = false;
page.appendChild(wrapper);

// 3. 将 Instance 移入 wrapper
wrapper.appendChild(inst);
inst.x = 0;  inst.y = 0;

// 4. 在 wrapper 内插透明 hotspot Frame
const hotspot = figma.createFrame();
hotspot.name = "hotspot / <按钮名> → <目标页>";
hotspot.x = <relX>;  hotspot.y = <relY>;
hotspot.resize(<ctaWidth>, <ctaHeight>);
hotspot.fills = [];
hotspot.clipsContent = false;
wrapper.appendChild(hotspot);

// 5. 在 hotspot 上设置 reaction
await hotspot.setReactionsAsync([{
  trigger: { type: 'ON_CLICK' },
  actions: [{
    type: 'NODE',
    destinationId: '<dest_frame_id>',
    navigation: 'NAVIGATE',
    transition: { type: 'DISSOLVE', easing: { type: 'EASE_OUT' }, duration: 0.3 }
  }]
}]);
```

> **结果**：Prototype 模式下只有 Sign In 按钮区域可点击，其他区域不触发跳转。

---

## 执行时机

在 Construction skill 的「阶段 N — 出图」完成、截图验证之前执行：

1. 拿到新 frame 的 nodeId
2. 按本页面对应的 `PAGE_KEYWORDS` 和 `DEST_LABEL` 填入模板
3. 执行 `use_figma`
4. 标注完成后再截图验证（标注应出现在截图里）
