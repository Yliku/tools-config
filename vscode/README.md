操作系统：Windows 11

### 安装步骤
创建配置文件的软链接，指向Git仓库的配置，以管理员身份打开CMD执行命令
```bash
mklink "%APPDATA%\Code\User\settings.json" D:\www\02config\vscode\settings.json
mklink "%APPDATA%\Code\User\keybindings.json" D:\www\02config\vscode\keybindings.json
mklink /D "%APPDATA%\Code\User\snippets" D:\www\02config\vscode\snippets  // 目录需要加 /D 参数
```

