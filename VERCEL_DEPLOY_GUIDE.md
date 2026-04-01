# Vercel 部署指南 - 一步步操作

## 前提条件

- 已安装 Node.js (v16+)
- 有 GitHub 账号
- 域名 `autonexus-ai.com` 已注册

---

## 步骤 1: 创建 GitHub 仓库 (5分钟)

### 1.1 初始化 Git 仓库

```bash
cd /home/admin/.openclaw/workspace/output/autonexus-ai/website

# 初始化 git
git init

# 添加所有文件
git add .

# 提交
git commit -m "Initial website launch"
```

### 1.2 创建 GitHub 仓库

1. 访问 https://github.com/new
2. 仓库名称: `autonexus-ai`
3. 选择 Public（免费）
4. 点击 **Create repository**

### 1.3 推送代码到 GitHub

```bash
# 添加远程仓库（替换为你的用户名）
git remote add origin https://github.com/YOUR_USERNAME/autonexus-ai.git

# 推送代码
git push -u origin main
```

---

## 步骤 2: 安装 Vercel CLI (2分钟)

```bash
# 全局安装 Vercel CLI
npm install -g vercel

# 验证安装
vercel --version
```

---

## 步骤 3: 登录 Vercel (2分钟)

```bash
# 登录 Vercel（会打开浏览器验证）
vercel login

# 按提示完成登录
```

---

## 步骤 4: 部署网站 (3分钟)

```bash
# 进入网站目录
cd /home/admin/.openclaw/workspace/output/autonexus-ai/website

# 首次部署
vercel

# 按提示回答：
# ? Set up and deploy "~/autonexus-ai/website"? [Y/n] → Y
# ? Which scope do you want to deploy to? → 选择你的账号
# ? Link to existing project? [y/N] → N
# ? What's your project name? [autonexus-ai] → 回车确认
# ? In which directory is your code located? [./] → 回车确认
```

部署完成后，会显示类似：
```
🔍  Inspect: https://vercel.com/username/autonexus-ai/xxxxx
✅  Production: https://autonexus-ai.vercel.app
```

---

## 步骤 5: 绑定自定义域名 (5分钟)

### 5.1 Vercel 控制台添加域名

```bash
# 添加域名
vercel domains add autonexus-ai.com
```

### 5.2 域名服务商配置 DNS

登录你的域名服务商（如阿里云、腾讯云、GoDaddy），添加以下记录：

| 类型 | 主机记录 | 记录值 | TTL |
|------|----------|--------|-----|
| A | @ | 76.76.21.21 | 600 |
| A | www | 76.76.21.21 | 600 |

> **Vercel IP 地址**：`76.76.21.21`

### 5.3 等待生效

- DNS 生效时间：几分钟到几小时
- SSL 证书：Vercel 自动申请和配置

---

## 步骤 6: 验证部署 (2分钟)

```bash
# 检查部署状态
vercel --version

# 打开网站
vercel open
```

访问以下地址验证：
- ✅ https://autonexus-ai.vercel.app (Vercel 默认域名)
- ✅ https://autonexus-ai.com (你的自定义域名)
- ✅ https://www.autonexus-ai.com (www 子域名)

---

## 快速命令参考

| 命令 | 作用 |
|------|------|
| `vercel` | 部署到预览环境 |
| `vercel --prod` | 部署到生产环境 |
| `vercel open` | 打开网站 |
| `vercel logs` | 查看日志 |
| `vercel domains add autonexus-ai.com` | 添加域名 |

---

## 更新网站

修改代码后重新部署：

```bash
cd /home/admin/.openclaw/workspace/output/autonexus-ai/website

# 修改文件...

# 提交更改
git add .
git commit -m "更新网站内容"
git push

# 自动部署（已连接 GitHub 后，推送即自动部署）
# 或手动部署
vercel --prod
```

---

## 故障排查

### 问题 1: 域名无法访问

```bash
# 检查 DNS 配置
vercel domains inspect autonexus-ai.com

# 确认 A 记录指向 76.76.21.21
```

### 问题 2: SSL 证书错误

- Vercel 自动申请 SSL，等待 1-24 小时
- 或手动触发：Vercel 控制台 → Project Settings → Domains → Refresh

### 问题 3: 404 错误

- 检查文件是否在仓库根目录
- 确认 `index.html` 存在

---

## 预计时间

| 步骤 | 时间 |
|------|------|
| 创建 GitHub 仓库 | 5分钟 |
| 安装 Vercel CLI | 2分钟 |
| 登录 Vercel | 2分钟 |
| 部署网站 | 3分钟 |
| 绑定域名 | 5分钟 |
| 验证 | 2分钟 |
| **总计** | **约 20 分钟** |

---

## 需要帮助？

Vercel 文档：https://vercel.com/docs

---
*创建时间: 2026-04-01*
