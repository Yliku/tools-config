# 我的配置文件仓库
## 增加新的配置文件流程，以下均以管理员身份运行CMD来执行命令
1. 进入仓库目录并创建文件夹
```bash
cd /d D:\www\tools-config\ && mkdir sublimeText 					// 必须加/d参数才能切换到不同的系统盘
```

2. 移动配置文件到Git仓库
- Sublime Text
```bash
move "%APPDATA%\Sublime Text\Packages\User" D:\www\tools-config\sublimeText\User

// 等价下面的两步：
cd /d "C:\Users\Yliku\AppData\Roaming\Sublime Text\Packages\"  	// Sublime Text文件夹有空格所以要加双引号
move .\User\ D:\www\tools-config\sublimeText\User
```

- VScode
	- 只需要监听目录下的这几个文件：settings.json ， keybindings.json ， snippets/
```bash
move "%APPDATA%\Code\User\settings.json" D:\www\tools-config\vscode\settings.json
move "%APPDATA%\Code\User\keybindings.json" D:\www\tools-config\vscode\keybindings.json
robocopy "%APPDATA%\Code\User\snippets" D:\www\tools-config\vscode\snippets /E 			// move命令会拒绝访问
```

3. 创建配置文件的软链接，指向Git仓库的配置
```bash
rmdir /s /q "%APPDATA%\Sublime Text\Packages\User"  // 删除旧文件夹（应用到新项目时需要）
mklink /D "%APPDATA%\Sublime Text\Packages\User" D:\www\tools-config\sublimeText\User	// /D代表文件夹，不加/D代表文件，在 Packages下面创建一个叫User的“链接” 
```

4. 验证是否成功
```bash
dir "%APPDATA%\Sublime Text\Packages"
```
看到：User [D:\www\tools-config\sublimeText\User] 👉 说明已经是链接，不是普通文件夹


## 用CMD来跑流程的原因：
1. Powershell的命令太复杂；
2. 用右键选择在此处打开 linux shell，wsl里面跑`ln`命令时，跨系统链接又有坑：Windows 和 WSL 的 symlink 不完全一致
	1. WSL 创建的链接：`ln -s`  👉 是 Linux symlink
	2. Windows 创建的链接，如下：👉 是 Windows symbolic link
	```bash
		New-Item -ItemType SymbolicLink
		mklink
	```
⚠️互相不一定完全兼容，所以推荐用CMD来跑流程！

## `move .\User\ D:\www\tools-config\sublimeText` 命令的坑
- 情况1：	D:\www\tools-config\sublimeText 的sublimeText文件夹已存在
   - 结果是：	D:\www\tools-config\sublimeText\User\	← 被移动进去
- 情况2：	D:\www\tools-config\sublimeText 的sublimeText文件夹不存在
   - 结果是：	D:\www\tools-config\sublimeText\   		← sublimeText文件夹实际就是 User内容
