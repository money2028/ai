# AutoNexus AI 网站部署指南

## 概述

这是一个静态营销网站，包含：
- 响应式HTML/CSS/JavaScript
- SEO优化（Schema.org标记）
- 移动端适配
- 表单处理（需后端集成）

## 部署要求

- 域名：autonexus-ai.com（已注册）
- 服务器：任何支持静态文件的Web服务器
- SSL证书：必需（HTTPS）

## 部署选项

### 选项1：Vercel（推荐，免费）

```bash
# 安装Vercel CLI
npm i -g vercel

# 进入网站目录
cd /home/admin/.openclaw/workspace/output/autonexus-ai/website

# 部署
vercel --prod

# 绑定自定义域名
vercel domains add autonexus-ai.com
```

### 选项2：Cloudflare Pages（推荐，免费）

1. 将代码推送到GitHub仓库
2. 登录Cloudflare Dashboard
3. 进入 Pages → Create a project
4. 连接GitHub仓库
5. 构建设置：
   - Build command: （留空，静态站点）
   - Build output directory: /
6. 添加自定义域名：autonexus-ai.com

### 选项3：GitHub Pages（免费）

```bash
# 创建GitHub仓库
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourusername/autonexus-ai.git
git push -u origin main

# 启用GitHub Pages
# 1. 进入仓库 Settings → Pages
# 2. Source: Deploy from a branch
# 3. Branch: main / (root)
# 4. 添加CNAME文件内容为: autonexus-ai.com
```

### 选项4：阿里云OSS + CDN（国内推荐）

```bash
# 1. 创建OSS Bucket
# 2. 开启静态网站托管
# 3. 上传文件
ossutil cp -r ./website oss://autonexus-ai/

# 4. 配置CDN加速
# 5. 绑定域名并配置HTTPS
```

### 选项5：自建服务器（VPS）

```bash
# 使用Nginx
sudo apt update
sudo apt install nginx

# 上传网站文件到 /var/www/autonexus-ai/
sudo mkdir -p /var/www/autonexus-ai
sudo cp -r website/* /var/www/autonexus-ai/

# 配置Nginx
sudo nano /etc/nginx/sites-available/autonexus-ai
```

Nginx配置：
```nginx
server {
    listen 80;
    server_name autonexus-ai.com www.autonexus-ai.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name autonexus-ai.com www.autonexus-ai.com;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    root /var/www/autonexus-ai;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    # Gzip压缩
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml;

    # 缓存静态资源
    location ~* \.(css|js|png|jpg|jpeg|gif|ico|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

```bash
# 启用配置
sudo ln -s /etc/nginx/sites-available/autonexus-ai /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx

# 配置SSL（使用Certbot）
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d autonexus-ai.com -d www.autonexus-ai.com
```

## DNS配置

在域名服务商处添加DNS记录：

| 类型 | 主机记录 | 记录值 | TTL |
|------|----------|--------|-----|
| A | @ | 服务器IP | 600 |
| A | www | 服务器IP | 600 |
| CNAME | * | autonexus-ai.com | 600 |

如果是Cloudflare/Vercel，按平台要求配置DNS。

## 部署检查清单

- [ ] 域名解析已生效
- [ ] SSL证书已配置
- [ ] 网站文件已上传
- [ ] HTTPS访问正常
- [ ] 移动端显示正常
- [ ] 表单提交测试
- [ ] SEO标记验证
- [ ] 性能优化（Gzip、缓存）
- [ ] 网站地图提交

## 性能优化

### 已实施
- ✅ CSS/JS压缩
- ✅ 图片优化（使用WebP）
- ✅ 响应式设计
- ✅ 懒加载（Intersection Observer）
- ✅ 动画优化

### 可选优化
- [ ] CDN加速（Cloudflare/阿里云CDN）
- [ ] 图片CDN（Cloudinary/Imgix）
- [ ] 资源预加载
- [ ] Service Worker缓存

## 监控

推荐使用：
- **Uptime监控**: UptimeRobot（免费）
- **性能监控**: Google PageSpeed Insights
- **SEO监控**: Google Search Console
- **流量分析**: Google Analytics 4

## 更新部署

```bash
# 修改代码后重新部署

# Vercel
vercel --prod

# GitHub Pages
git add .
git commit -m "Update website"
git push

# 手动部署
rsync -avz --delete website/ user@server:/var/www/autonexus-ai/
```

## 故障排查

### 常见问题

1. **页面404**
   - 检查文件路径
   - 确认Nginx/Apache配置
   - 检查URL重写规则

2. **CSS/JS未加载**
   - 检查文件路径（相对路径）
   - 确认MIME类型
   - 清除浏览器缓存

3. **HTTPS证书错误**
   - 检查证书有效期
   - 确认证书链完整
   - 检查域名匹配

4. **表单提交失败**
   - 需要后端API支持
   - 或使用表单服务（Formspree/Getform）

## 联系方式

部署问题联系：
- 邮箱：hello@autonexus-ai.com

---
*部署指南版本: 1.0*
*更新日期: 2026-04-01*
