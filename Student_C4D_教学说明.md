# C4D 教学说明

> 如何安装、复现和使用本地大模型地图 Agent
> 面向：零基础同学也能跟着做

---

## 一、快速开始（5 分钟跑通）

### Step 1：安装 Ollama（2 分钟）

**Windows 用户**：
1. 打开浏览器，访问 https://ollama.com/download
2. 点击 "Download for Windows"
3. 下载完成后双击安装包，一路下一步
4. 安装完成后，Ollama 会自动在后台运行（任务栏右下角有羊驼图标）

**Mac 用户**：
1. 访问 https://ollama.com/download
2. 点击 "Download for macOS"
3. 打开下载的 .dmg 文件，把 Ollama 拖到 Applications
4. 从启动台打开 Ollama

**Linux 用户**：
打开终端，输入一行命令：
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

### Step 2：下载 Gemma 4 模型（2-10 分钟，取决于网速）

打开终端（Windows 按 Win+R 输入 cmd，Mac 按 Command+空格输入 terminal），输入：

```bash
ollama pull gemma4:e4b
```

> 💡 **不知道选哪个模型？**
> - 电脑内存 8GB → 用 `gemma4:e2b`（最小，一定能跑）
> - 电脑内存 16GB → 用 `gemma4:e4b`（推荐，效果好）
> - 电脑有独立显卡 16GB+ 显存 → 可以试试 `gemma4:26b`

下载进度会显示在终端里，耐心等待。下载完成后输入 `ollama list` 确认模型已安装。

### Step 3：安装 Python 依赖（1 分钟）

```bash
pip install requests folium
```

> 如果提示 pip 命令不存在，说明 Python 没装。先去 https://www.python.org/downloads/ 安装 Python 3，安装时记得勾选 "Add Python to PATH"。

### Step 4：运行地图生成器（10-30 秒）

把 `Student_C4D_Agent技能` 文件夹放到一个你找得到的地方（比如桌面），然后在终端里：

```bash
# 进入脚本所在目录
cd Desktop/Student_C4D_Agent技能/scripts

# 运行（默认调用本地 gemma4:e4b）
python sias_map_agent.py
```

> Windows 用户如果 `python` 不行，试试 `python3` 或 `py`。

### Step 5：查看地图

运行成功后，会在当前目录生成 `Student_C4D_map.html`。双击这个文件，就会在浏览器里打开交互式地图！

---

## 二、常见问题（FAQ）

### Q1：运行时提示"无法连接 Ollama 服务"

**原因**：Ollama 没有启动。

**解决**：
- Windows：检查任务栏右下角有没有羊驼图标，没有的话从开始菜单打开 Ollama
- Mac：从启动台打开 Ollama
- Linux：终端输入 `ollama serve`，保持窗口开着

然后重新运行脚本。

### Q2：提示"model 'gemma4:e4b' not found"

**原因**：模型还没下载，或者下载的模型名字不对。

**解决**：
1. 输入 `ollama list` 看看已安装的模型
2. 如果没有 gemma4，输入 `ollama pull gemma4:e4b` 重新下载
3. 如果模型名字不同（比如叫 gemma4:26b），运行时指定：`python sias_map_agent.py --model gemma4:26b`

### Q3：模型下载太慢

**原因**：网络问题，Ollama 默认从国外服务器下载。

**解决**：
- 换个时间下载（凌晨网速快）
- 用手机热点试试
- 选择更小的模型 `gemma4:e2b`（下载快，占用小）
- 搜索"Ollama 国内镜像"找加速方法

### Q4：生成的地图打开是空白的

**原因**：地图底图（OpenStreetMap）需要联网加载。

**解决**：
- 确保电脑联网
- 等几秒钟，底图加载需要一点时间
- 如果还是空白，试试换个浏览器（Chrome/Edge/Firefox）

### Q5：LLM 生成的地点坐标不对

**原因**：大模型可能产生"坐标幻觉"，生成大致正确但具体位置有偏差的坐标。

**解决**：
- 这是正常现象，脚本内置了数据验证器会标记异常坐标
- 可以用 `--offline` 模式使用经过人工核实的内置数据
- 或者在生成后手动调整 HTML 中的坐标

### Q6：我不想装 Ollama，能直接用吗？

**可以！** 使用离线模式：
```bash
python sias_map_agent.py --offline
```
离线模式使用内置的 SIAS 大学真实数据，不需要任何本地模型，直接生成地图。但这样就没有展示"本地大模型 Agent"的能力了，建议还是装上 Ollama 跑一次在线模式。

### Q7：怎么截图证明是在本地跑的？

运行在线模式时，截图需要包含以下信息：
1. **终端窗口**：显示运行的命令、模型名称、生成过程
2. **Ollama 信息**：可以另开一个终端输入 `ollama list` 显示已安装的模型
3. **地图结果**：浏览器中打开生成的 HTML 地图
4. **设备信息**：可以在截图中包含任务管理器/活动监视器显示内存占用

建议拼一张图：左边终端+右边地图，这样一张图就能证明全部。

---

## 三、进阶用法

### 3.1 生成其他地点的地图

脚本默认生成 SIAS 大学的地图。要生成其他地点，需要修改代码中的 prompt。打开 `scripts/sias_map_agent.py`，找到 `generate_locations` 方法中的 `user_prompt`，把 "SIAS University" 改成你想要的地点（比如 "Zhengzhou University"、"Beijing Forbidden City"）。

同时修改 `generate_map` 中的 `center_lat` 和 `center_lon` 为目标地点的坐标。

### 3.2 调整地图样式

在 `generate_map` 函数中可以修改：
- `tiles="OpenStreetMap"` → 改成 `"CartoDB positron"`（更简洁的白底风格）
- `zoom_start=15` → 数字越大初始缩放越近
- 标记颜色：修改 `category_colors` 字典

### 3.3 打包为 .skill 文件

这个技能可以打包为标准的 Claude Skill 格式（.skill 文件本质是 tar.gz 压缩包）：

```bash
cd Student_C4D_Agent技能
tar -czf ../c4d-local-llm-map-agent.skill .
```

然后就可以在 Claude 中安装使用了。

---

## 四、优化技巧

### 4.1 提升生成速度
- 使用更小的模型（E2B 比 E4B 快）
- 减少生成地点数量（`--locations 5`）
- 有 GPU 的话确保 Ollama 使用 GPU（自动检测）

### 4.2 提升输出质量
- 使用更大的模型（26B/31B 质量更高）
- 使用 Q8 量化（比 Q4 质量好）
- 修改 System Prompt，增加更详细的输出要求

### 4.3 常见坑
- **不要在路径中有中文**：Python 脚本所在路径尽量全英文，避免编码问题
- **关闭代理**：运行 Ollama 时如果开了系统代理，可能导致连接异常，建议关闭
- **耐心等待**：第一次运行模型需要加载，可能需要 10-30 秒，不要以为卡死了

---

## 五、检查清单（提交前确认）

- [ ] Ollama 已安装并启动
- [ ] Gemma 4 模型已下载（`ollama list` 能看到）
- [ ] Python 依赖已安装（requests + folium）
- [ ] 在线模式运行成功，生成了地图 HTML
- [ ] 地图在浏览器中正常显示
- [ ] 截图包含：模型名称 + 设备信息 + 推理过程 + 地图结果
- [ ] 所有文档已填写（验证报告中的实际测试数据）
- [ ] 文件按规范命名（姓名_C4D_xxx）
