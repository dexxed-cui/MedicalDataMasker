# MedicalDataMasker (医疗凭证智能脱敏系统)

本项目是一个专注于 **Python 高并发处理**、**多端工程适配**与**自动化图像坐标对齐**的医疗隐私脱敏工具。

⚠️ **重要声明**：本项目纯粹用于个人 Python 编程技术的深度学习、踩坑记录与作品集展示，不涉及任何商业用途，不上传、不存储任何真实的患者隐私数据。

---

## 🚀 核心技术亮点与工程突破

在开发本项目期间，为了克服多端环境限制与大文件分发问题，物理攻克了以下三个工业级工程痛点：

### 1. 跨平台多端轻量化适配 (iPadOS 纯终端交互)
- **挑战**：在 iPadOS (iSh Alpine Linux) 环境下，传统的图形化外壳（如 Tkinter）无法正常渲染，导致程序彻底瘫痪。
- **解法**：强行剥离了所有重量级 UI 依赖，将核心打码算法重构为纯命令行并发 Daemon 守卫进程；巧妙利用 iOS “文件” App 的**物理拖拽与挂载机制**，实现了无界面、高并发的轻量化文件交互方案。

### 2. 图像预处理与坐标系物理对齐 (EXIF 旋转修正)
- **挑战**：用户使用移动设备拍摄的病历、发票通常自带 EXIF 旋转障眼法，导致本地画笔的绝对坐标系与云端智能 OCR 识别的坐标系发生物理错位，打码经常“对不准”。
- **解法**：在图像预处理阶段引入了 EXIF 偏转角物理矫正算法，强行拉平图像物理时空，确保本地打码黑杠能够 100% 精准覆盖敏感隐私区域。

### 3. 工业级大文件分发架构 (规避 GitHub 传输锁)
- **挑战**：由于编译后的 macOS 标准拖拽安装镜像 (`.dmg`) 和 `.app.zip` 压缩包体积较大（接近 80MB），直接走常规的 `git push` 或网页端普通 HTTPS 通道会由于单文件 25MB 限流锁及国际网络抖动，引发服务器无情掐断连接 (`unexpected disconnect`)。
- **解法**：对齐了标准开源大厂的发布流水线。本地代码区仅保留纯净的源码与 `.gitignore` 隔离规则，大文件成品则通过构建 `Git Tag`，强行开辟 GitHub 专用的 **Release VIP 通道**，利用其专有的 CDN 实现了稳定、高效的二进制分发。

---

## 🛠️ 本地环境构建与依赖解耦

本项目在开发时彻底规避了全局 Conda 环境带来的依赖污染，建议在完全隔离的本地虚拟环境中运行：

```bash
# 1. 克隆本项目至本地 Documents 干净目录
git clone [https://github.com/dexxed-cui/MedicalDataMasker.git](https://github.com/dexxed-cui/MedicalDataMasker.git)
cd MedicalDataMasker

# 2. 构建纯净的本地沙盒环境（隔离全局 Conda 环境）
python3 -m venv venv_masker
source venv_masker/bin/activate

# 3. 安装经物理对齐的轻量化依赖
pip install --upgrade pip
pip install requests pillow urllib3 chardet
