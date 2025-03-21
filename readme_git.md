# Git与GitHub使用指南

这篇指南将帮助你完成从安装Git到将项目上传至GitHub的全过程。

## 1. 下载安装Git

1. 访问Git官网下载页面：https://git-scm.com/downloads
2. 选择适合你操作系统的版本下载
3. 运行安装程序，大部分选项保持默认即可
   - 安装过程中可以选择文本编辑器，推荐选择VS Code或Notepad++
   - 建议选择"Use Git from the Windows Command Prompt"选项

## 2. 注册GitHub账号

1. 访问GitHub官网：https://github.com
2. 点击"Sign up"并填写必要信息
3. 验证邮箱地址

## 3. 配置Git

安装Git后，需要设置你的用户名和邮箱，打开命令行工具（如PowerShell或CMD）：

```bash
# 设置全局用户名
git config --global user.name "你的用户名"

# 设置全局邮箱
git config --global user.email "你的邮箱"

# 查看设置
git config --list
```

## 4. 创建SSH密钥（推荐）

SSH密钥可以让你与GitHub通信时不需要每次都输入密码：

```bash
# 生成SSH密钥，按提示操作
ssh-keygen -t rsa -b 4096 -C "你的邮箱"
```

当执行上述命令后，会出现以下提示：

```
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\用户名/.ssh/id_rsa):
```

这是询问你想将SSH密钥保存在哪个位置。括号中的路径是默认保存位置。如果你同意使用默认位置，直接按Enter键即可；如果想指定其他位置，输入完整路径后按Enter。

然后会提示你输入密码（可选）：

```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

这是为你的SSH密钥设置密码。为了安全，建议设置密码，但如果希望无需密码直接使用，可以直接按Enter跳过。

```bash
# 启动ssh-agent
eval "$(ssh-agent -s)"  # Linux/Mac
# 或在Windows PowerShell中：
# Start-Service ssh-agent
```

**Windows上启动ssh-agent可能遇到的问题**：

如果在Windows上执行 `Start-Service ssh-agent`时出现"无法启动服务"的错误，可能是因为OpenSSH服务未安装或未启用。解决方法：

1. **安装OpenSSH**：

   - 打开"设置" > "应用" > "可选功能"
   - 点击"添加功能"，搜索并安装"OpenSSH 客户端"和"OpenSSH 服务器"
2. **启用并设置服务**：

   - 按Win+R，输入 `services.msc`打开服务管理器
   - 找到"OpenSSH Authentication Agent"服务
   - 双击，将启动类型改为"自动"，然后点击"启动"按钮
3. **如果仍然无法启动**，可以不使用ssh-agent，直接使用密钥：

   - 确保SSH密钥在默认位置(`~/.ssh/id_rsa`)
   - 在使用Git进行SSH连接时，系统会自动查找默认位置的密钥
   - 如果设置了密码，每次使用时需要输入密码

```bash
# 添加SSH私钥
ssh-add ~/.ssh/id_rsa
```

**Windows路径问题解决方案**：

当在Windows上使用SSH命令时，可能会遇到路径识别问题。如果 `ssh-add ~/.ssh/id_rsa`返回"No such file or directory"错误，请尝试以下方法：

1. **使用绝对路径**：

   ```bash
   # 替换为你的Windows用户名
   ssh-add C:/Users/用户名/.ssh/id_rsa
   # 或使用反斜杠 (需要转义)
   ssh-add C:\\Users\\用户名\\.ssh\\id_rsa
   ```
2. **验证密钥位置**：

   - 运行 `ssh-keygen`命令时会显示密钥保存的确切位置
   - 例如："Your identification has been saved in C:\Users\32185/.ssh/id_rsa"
   - 使用这个确切路径
3. **使用环境变量**：

   ```bash
   ssh-add $env:USERPROFILE/.ssh/id_rsa  # PowerShell
   # 或在CMD中
   # ssh-add %USERPROFILE%/.ssh/id_rsa
   ```
4. **如果ssh-agent服务已运行但仍有问题**：

   - 确认SSH密钥文件确实存在（使用文件资源管理器导航查看）
   - 检查文件权限，确保当前用户有读取权限

然后将SSH公钥添加到GitHub：

1. 复制公钥内容：`cat ~/.ssh/id_rsa.pub` 或使用记事本打开该文件
2. 在GitHub网站，点击右上角头像 -> Settings -> SSH and GPG keys
3. 点击"New SSH key"，粘贴公钥内容并保存

## 5. 在GitHub上创建新仓库

1. 登录GitHub，点击右上角"+"图标，选择"New repository"
2. 填写仓库名称、描述（可选）
3. 选择公开或私有
4. 不要勾选"Initialize this repository with a README"（如果你要上传已有项目）
5. 点击"Create repository"

## 6. 将本地项目上传到GitHub

### 方法一：上传现有项目

如果你已有本地项目：

```bash
# 进入项目目录
cd 你的项目路径

# 初始化Git仓库
git init

# 添加所有文件到暂存区
git add .

# 提交更改
git commit -m "初始提交"

# 添加远程仓库地址
git remote add origin git@github.com:用户名/仓库名.git
# 或者使用HTTPS（需要每次输入用户名密码）
# git remote add origin https://github.com/用户名/仓库名.git

# 推送到GitHub
git push -u origin master  # 或 main，取决于你的默认分支名
```

### 方法二：克隆已有仓库

如果你想从GitHub获取已有项目：

```bash
# 克隆仓库
git clone git@github.com:用户名/仓库名.git
# 或者使用HTTPS
# git clone https://github.com/用户名/仓库名.git

# 进入项目目录
cd 仓库名
```

## 7. 日常工作流程

每次修改代码后的上传流程：

```bash
# 查看文件状态
git status

# 添加修改的文件
git add .  # 添加所有修改
# 或者
git add 文件名  # 添加指定文件

# 提交更改
git commit -m "更改说明"

# 推送到GitHub
git push
```

## 8. 常用Git命令

```bash
# 查看本地分支
git branch

# 创建并切换到新分支
git checkout -b 新分支名

# 切换分支
git checkout 分支名

# 合并分支
git merge 分支名

# 拉取远程更新
git pull

# 查看提交历史
git log

# 撤销未提交的修改
git checkout -- 文件名

# 撤销已暂存的修改
git reset HEAD 文件名
```

## 9. 处理冲突

当多人协作时可能会遇到冲突，需要手动解决：

1. 执行 `git pull`时遇到冲突
2. 打开冲突文件，会看到类似以下内容：
   ```
   <<<<<<< HEAD
   你的代码
   =======
   别人的代码
   >>>>>>> 分支名
   ```
3. 修改文件，保留你想要的内容并删除标记符
4. 重新添加、提交并推送：
   ```bash
   git add .
   git commit -m "解决冲突"
   git push
   ```

## 10. .gitignore文件

创建 `.gitignore`文件可以告诉Git忽略某些文件：

```
# 示例.gitignore
node_modules/
.env
*.log
.DS_Store
```

## 常见问题与解决方案

1. **推送被拒绝**：通常是因为远程有本地没有的更改，先执行 `git pull`再推送
2. **SSL证书问题**：执行 `git config --global http.sslVerify false`（不推荐在公共环境）
3. **凭据缓存**：Windows可使用 `git config --global credential.helper wincred`

希望这份指南能帮助你成功使用Git和GitHub！
