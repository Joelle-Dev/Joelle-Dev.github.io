# Git 认证问题解决方案

## 问题说明
GitHub 从 2021 年 8 月起不再支持密码认证，需要使用：
- Personal Access Token (PAT) 或
- SSH 密钥

---

## 方案一：使用 SSH（推荐）⭐

### 优点
- ✅ 更安全
- ✅ 一次配置，永久使用
- ✅ 无需每次输入密码

### 步骤

#### 1. 检查 SSH 密钥是否已添加到 GitHub

```bash
# 查看你的公钥
cat ~/.ssh/id_rsa.pub
```

#### 2. 如果没有 SSH 密钥，生成新的

```bash
# 生成 SSH 密钥（如果还没有）
ssh-keygen -t ed25519 -C "your_email@example.com"

# 或者使用 RSA（兼容性更好）
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

#### 3. 将 SSH 公钥添加到 GitHub

1. 复制公钥内容：
```bash
cat ~/.ssh/id_rsa.pub | pbcopy  # macOS
# 或
cat ~/.ssh/id_rsa.pub
```

2. 登录 GitHub → Settings → SSH and GPG keys → New SSH key
3. 粘贴公钥并保存

#### 4. 测试 SSH 连接

```bash
ssh -T git@github.com
```

如果看到 "Hi Joelle-Dev! You've successfully authenticated..." 说明成功。

#### 5. 将远程仓库 URL 改为 SSH

```bash
cd /Users/github/mozzieMIUMIU.github.io
git remote set-url origin git@github.com:Joelle-Dev/mozzieMIUMIU.github.io.git
```

#### 6. 验证

```bash
git remote -v
# 应该显示 git@github.com:... 而不是 https://...
```

---

## 方案二：使用 Personal Access Token (PAT)

### 步骤

#### 1. 创建 Personal Access Token

1. 登录 GitHub
2. Settings → Developer settings → Personal access tokens → Tokens (classic)
3. Generate new token (classic)
4. 设置权限（至少需要 `repo` 权限）
5. 生成并**复制 token**（只显示一次！）

#### 2. 使用 Token 推送

```bash
# 方法 1：在 URL 中使用 token
git remote set-url origin https://YOUR_TOKEN@github.com/Joelle-Dev/mozzieMIUMIU.github.io.git

# 方法 2：使用 Git Credential Manager（推荐）
# 推送时会提示输入用户名和密码
# 用户名：你的 GitHub 用户名
# 密码：粘贴你的 Personal Access Token
```

#### 3. 配置 Git Credential Helper（macOS）

```bash
# 使用 macOS Keychain 存储凭证
git config --global credential.helper osxkeychain

# 或者使用 cache（临时存储）
git config --global credential.helper cache
```

---

## 快速修复命令（SSH 方案）

```bash
# 1. 检查 SSH 密钥
cat ~/.ssh/id_rsa.pub

# 2. 测试 SSH 连接
ssh -T git@github.com

# 3. 切换到 SSH URL
cd /Users/github/mozzieMIUMIU.github.io
git remote set-url origin git@github.com:Joelle-Dev/mozzieMIUMIU.github.io.git

# 4. 验证
git remote -v

# 5. 测试推送
git push origin master
```

---

## 常见问题

### Q: SSH 连接失败？
A: 检查：
```bash
# 查看 SSH 配置
cat ~/.ssh/config

# 测试连接
ssh -vT git@github.com
```

### Q: 如何查看已保存的凭证？
A:
```bash
# macOS Keychain
git credential-osxkeychain get

# 或查看 Git 配置
git config --list | grep credential
```

### Q: 如何清除已保存的凭证？
A:
```bash
# macOS Keychain
git credential-osxkeychain erase
host=github.com
protocol=https

# 或删除缓存
git credential-cache exit
```

---

## 推荐配置

### 全局 Git 配置

```bash
# 设置用户名和邮箱
git config --global user.name "Joelle-Dev"
git config --global user.email "your_email@example.com"

# 使用 SSH（推荐）
git config --global url."git@github.com:".insteadOf "https://github.com/"

# 凭证存储（如果使用 HTTPS）
git config --global credential.helper osxkeychain
```

---

## 验证修复

```bash
# 检查远程 URL
git remote -v

# 测试推送（不会真的推送，只是测试连接）
git push --dry-run origin master
```

