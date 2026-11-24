# 僵尸连连看

一个离线、无框架的中文配对小游戏。默认棋盘为 6 行 × 6 列，共 18 对。HTML、CSS、JavaScript 全在 `index.html` 中，图案使用设备自带的 Emoji 字体。

## 怎么运行

1. 在 GitHub 仓库首页选择 **Code（代码）→ Download ZIP（下载 ZIP）**，解压到任意文件夹。已经拿到单独的 `index.html` 可以直接进入下一步。
2. 在 Windows 或 Mac 上双击 `index.html`，或用浏览器的“打开文件”选择它。地址以 `file://` 开头就表示打开的是本地副本。
3. 断网后刷新页面，棋盘仍会重新生成。无需账号、服务器、npm 或安装依赖。刷新和重新开局都会清除本局进度。

## 怎么玩

- 点击两张相同图案。连线只能水平或垂直穿过空格，允许直连、一个拐角或两个拐角，也可以走棋盘外侧的一圈空走廊。
- 再点已选牌可取消。配对失败时，后一张成为新的起点。配对成功会短暂显示连线，随后两张消失。剩余对数归零即通关。
- **提示一对**标出两张可消除的牌，由你亲自点选。**洗牌**只重排余牌，保留空格。出现死局时，游戏会自动调位并提示。**重新开局**立即恢复满盘。
- 键盘也可以玩。用 Tab 移动焦点，用 Enter 或空格激活按钮。

## 你需要亲自理解和修改的部分

| 找到的位置 | 亲手做什么 | 怎么确认自己理解了 |
| --- | --- | --- |
| HTML 的规则段落、CSS 的 `.tile.selected` | 改一句规则说明，改一个选中颜色 | 选中、取消、请求提示，能分辨三种状态 |
| `ROWS`、`COLS`、`ICONS` 和 `NAMES` | 改棋盘尺寸，或成对修改同一位置的图案和中文名称 | 当前实现要求行列均至少为 2，总格数为偶数。能解释 6 × 8 为何是 24 对 |
| `choose()`、`route()` 和 `ensureMove()` | 跟着一次点击读代码，再完成下面的外圈规则实验 | 能指出图案在哪里比较、障碍在哪里检查、为什么洗牌后总有一步可走 |

## 四次递进式 Git 提交练习

先在终端执行下面三行。每项练习只提交自己的那一处修改。编辑后保存，刷新本地页面验证，再运行对应命令。

~~~sh
git clone https://github.com/bamungtanggalvin-sudo/bamungtanggalvin159.git
cd bamungtanggalvin159
git switch -c practice/first-game
~~~

**1. 修改规则文案。** 在 `id="rules"` 段落末尾补一句“再点一次可取消”。点击同一张两次，确认文字与行为相符。第一次提交慢慢做，留意工作区与暂存区的区别。

~~~sh
git diff
git add index.html
git diff --cached
git commit -m "docs: 说明重复点击可取消选牌"
~~~

**2. 修改选中样式。** 在 `.tile.selected` 中换一种背景色。点选一张，再用“提示一对”，确认自己的颜色和提示色容易区分。

~~~sh
git add index.html
git commit -m "style: 调整选中牌的底色"
~~~

**3. 扩大棋盘。** 将 `COLS` 从 6 改为 8，保留 6 行。同步更新 README 首段的默认尺寸与对数。验证开始时显示 24 对，成功一次剩 23 对，洗牌后仍是 23 对。

~~~sh
git add index.html README.md
git commit -m "feat: 将默认棋盘扩为六行八列"
~~~

**4. 用实验读懂外圈寻路。** 临时把 `route()` 中枚举行、列的循环分别改成从 0 开始、以 `y < ROWS` 和 `x < COLS` 为条件。刷新页面，在浏览器开发者工具的 Console（控制台）执行下列探针。它只检查满盘的同一行两端之间有没有几何通路，图案是否相同由另一层判断。

~~~js
Boolean(route(0, COLS - 1))
~~~

限制外圈后应得到 `false`。撤销刚才的两处循环改动并刷新，原规则应得到 `true`。将两次结果和原因用自己的话补到 README，再提交。这次提交只保存实验结论。用 `git status --short` 确认仅 README 有变化。

~~~sh
git status --short
git add README.md
git commit -m "docs: 记录外圈寻路的对照实验"
git log --oneline -4
~~~

练习保存在本地分支。想在 GitHub 上保留这四次提交时，执行 `git push -u origin practice/first-game`。
