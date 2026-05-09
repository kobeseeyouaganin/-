# 🚀 每日科技热点日报系统

一个**全自动**的科技新闻收集和发送系统，每天早上9点精选全球最新的科技热点新闻发送到你的邮箱。

## ✨ 核心功能

✅ **自动爬取** - 从4个全球权威科技源实时抓取最新资讯  
✅ **智能去重** - 自动去除重复新闻，保证内容新鲜  
✅ **定时发送** - 每天早上9点准时发送精选20条新闻  
✅ **美观格式** - 专业HTML邮件排版，看起来棒极了  
✅ **数据持久化** - SQLite数据库本地存储，方便查询  
✅ **错误处理** - 完善的异常捕获和日志记录  

## 📰 支持的新闻源

| 源 | 类型 | 说明 |
|---|---|---|
| **HackerNews** | 技术社区 | 全球顶级程序员讨论的新闻 |
| **GitHub Trending** | 开源项目 | 热门开源项目排行 |
| **TechCrunch** | 科技新闻 | 硅谷创业和科技新闻 |
| **Reddit** | 讨论论坛 | r/technology 热门讨论 |

## 📂 项目结构

```
.
├── config/
│   └── settings.py           # ⚙️ 系统配置（邮箱、发送时间等）
├── core/
│   ├── database.py           # 📊 数据库管理（去重、存储）
│   ├── email_sender.py       # 📧 邮件发送模块
│   └── scheduler.py          # ⏰ 定时调度器
├── crawlers/
│   ├── base_crawler.py       # 爬虫基类
│   ├── hackernews_crawler.py # HackerNews爬虫
│   ├── github_trending_crawler.py  # GitHub爬虫
│   ├── techcrunch_crawler.py # TechCrunch爬虫
│   └── reddit_crawler.py     # Reddit爬虫
├── main.py                   # 🚀 主程序入口
├── requirements.txt          # 📦 依赖列表
└── README.md                 # 📖 项目说明
```

## 🚀 快速开始

### 1️⃣ 环境准备

```bash
# 克隆项目
git clone https://github.com/kobeseeyouaganin/-.git
cd -

# 创建虚拟环境
python -m venv venv
source venv/bin/activate  # Linux/Mac
# 或
venv\Scripts\activate     # Windows

# 安装依赖
pip install -r requirements.txt
```

### 2️⃣ 配置Gmail

#### 步骤 1: 启用Google两步验证
- 访问 [Google账户安全设置](https://myaccount.google.com/security)
- 启用"两步验证"

#### 步骤 2: 生成应用密码
- 在安全设置页面，找到 "应用密码"
- 选择"邮件"和"Windows电脑"
- 生成16位密码

#### 步骤 3: 修改配置
编辑 `config/settings.py`：
```python
GMAIL_ADDRESS = "your-email@gmail.com"       # 你的Gmail地址
GMAIL_PASSWORD = "xxxx xxxx xxxx xxxx"       # 16位应用密码
RECIPIENT_EMAIL = "your-email@gmail.com"    # 接收邮件的地址
SEND_TIME = time(9, 0)                       # 发送时间 (早上9点)
NEWS_COUNT = 20                              # 每次发送条数
```

### 3️⃣ 运行系统

```bash
# 启动定时任务（推荐）
python main.py start

# 或手动运行单个命令
python main.py crawl    # 立即爬取新闻
python main.py send     # 立即发送日报
python main.py stats    # 显示统计信息
```

## 📧 邮件示例

收到的邮件将包含：
- 📌 **标题** - 新闻标题，点击可直接跳转
- 🏷️ **来源标签** - 标识新闻来自哪个源
- 📝 **摘要** - 新闻内容摘要（150字）
- 📅 **发布日期** - 新闻发布时间
- 🎨 **精美排版** - 渐变色背景，响应式设计

## ⚙️ 高级配置

### 修改发送时间
编辑 `config/settings.py`：
```python
from datetime import time
SEND_TIME = time(8, 30)  # 改成8点30分
```

### 修改新闻条数
```python
NEWS_COUNT = 30  # 改成每次发送30条
```

### 启用代理（科学上网）
```python
USE_PROXY = True
PROXY_URL = "http://127.0.0.1:7890"  # 改成你的代理地址
```

### 添加新的爬虫源
1. 在 `crawlers/` 目录创建新文件，继承 `BaseCrawler`
2. 实现 `parse()` 和 `get_url()` 方法
3. 在 `main.py` 中添加到 `scheduler.add_crawler()`

## 📊 命令行界面

```bash
python main.py start              # 🚀 启动24/7定时任务
python main.py crawl              # 🔍 立即爬取新闻
python main.py send               # 📧 立即发送日报
python main.py stats              # 📊 显示统计信息
python main.py clean --days 30    # 🧹 清理30天前的旧新闻
python main.py help               # ❓ 显示帮助信息
```

## 🔧 故障排除

### ❌ 邮件发送失败

**错误**: `Gmail authentication failed`
- ✅ 确保启用了Google两步验证
- ✅ 确保生成了16位应用密码（非账户密码）
- ✅ 检查 `config/settings.py` 中的密码是否正确

**错误**: `Connection refused`
- ✅ 检查网络连接
- ✅ 检查防火墙是否阻止了SMTP端口(587)
- ✅ 尝试启用代理

### ❌ 爬虫无法获取数据

- ✅ 检查网络连接
- ✅ 某些网站可能需要代理，可在 `config/settings.py` 中启用代理
- ✅ 网站可能更改了HTML结构，需要更新爬虫代码

### ❌ 数据库错误

- ✅ 确保 `data/` 目录存在且可写
- ✅ 尝试删除 `data/news.db` 文件重新初始化

## 📝 日志

所有日志记录在 `logs/news_crawler.log`，包含：
- 爬虫运行情况
- 邮件发送状态
- 错误信息和调试信息

查看日志：
```bash
tail -f logs/news_crawler.log      # Linux/Mac
Get-Content logs/news_crawler.log  # PowerShell
type logs/news_crawler.log         # CMD
```

## ☁️ 云服务部署

### 在Linux服务器上后台运行

```bash
# 使用 nohup 后台运行
nohup python main.py start > output.log 2>&1 &

# 或使用 screen
screen -S news-crawler python main.py start

# 查看进程
ps aux | grep main.py

# 停止程序
pkill -f "python main.py start"
```

### 使用Systemd服务（推荐）

创建 `/etc/systemd/system/tech-news.service`：
```ini
[Unit]
Description=Technology News Daily Reporter
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu/tech-news
ExecStart=/home/ubuntu/tech-news/venv/bin/python main.py start
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

启动服务：
```bash
sudo systemctl start tech-news
sudo systemctl enable tech-news       # 开机自启
sudo systemctl status tech-news       # 查看状态
```

### Docker部署

创建 `Dockerfile`：
```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY . .
RUN pip install -r requirements.txt

CMD ["python", "main.py", "start"]
```

构建并运行：
```bash
docker build -t tech-news-daily .
docker run -d --name tech-news tech-news-daily
```

## 🎯 工作流程

```
┌─────────────────────────────────┐
│      启动定时任务               │
├─────────────────────────────────┤
│  ├─ 每6小时运行爬虫             │
│  │  ├─ HackerNews爬虫           │
│  │  ├─ GitHub爬虫               │
│  │  ├─ TechCrunch爬虫           │
│  │  └─ Reddit爬虫               │
│  │                              │
│  ├─ 数据库去重存储              │
│  │                              │
│  └─ 每天9点发送日报             │
│     ├─ 获取最新20条新闻         │
│     ├─ 生成HTML邮件             │
│     ├─ 发送到Gmail              │
│     └─ 标记为已发送             │
│                                 │
└─────────────────────────────────┘
```

## 💡 使用建议

1. **首次运行** - 先运行 `python main.py crawl` 测试爬虫是否正常工作
2. **测试邮件** - 运行 `python main.py send` 测试邮件功能
3. **后台运行** - 使用 `nohup python main.py start > output.log 2>&1 &`
4. **定期检查** - 定期查看日志确保系统正常运行
5. **定期清理** - 定期运行 `python main.py clean` 清理旧新闻

## 🤝 贡献

欢迎提交Issue和Pull Request！

## 📄 许可证

MIT License - 详见 LICENSE 文件

---

**最后更新**: 2026-05-09  
**版本**: v1.0  
**状态**: ✅ 完成基础功能  

**需要帮助?** 提交Issue或查看 [项目主页](https://github.com/kobeseeyouaganin/-)
