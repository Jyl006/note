
**GitHub 连接失败排错笔记：从 HTTPS 到 SSH (端口 443)**

**问题描述：**

在尝试使用 `git clone https://github.com/moonify-J/note.git` 命令克隆 GitHub 仓库时失败。

**遇到的错误及排错过程：**

1. **错误一：HTTPS 连接失败 (端口 443)**
    
    - **错误信息：**
        
        ```
        fatal: unable to access 'https://github.com/moonify-J/note.git/': Failed to connect to github.com port 443 after 2698 ms: Could not connect to server
        ```
        
    - **分析：** 无法连接 `github.com` 的 443 端口（HTTPS 默认端口）。通常是网络不通、本地防火墙或网络环境阻止了对 GitHub 的访问。
    - **初步排查：** 检查网络连接、本地 Windows 防火墙和可能的第三方安全软件设置。虽然进行了防火墙检查，但问题似乎未解决，决定尝试使用 SSH 协议。
2. **尝试改用 SSH 协议，配置 SSH Key**
    
    - 生成 SSH Key (`ssh-keygen`)，并将公钥 (`id_rsa.pub`) 添加到 GitHub 账号设置中。
3. **错误二：SSH 连接被拒绝 (端口 22)**
    
    - **测试命令：** `ssh -T git@github.com`
    - **错误信息：**
        
        ```
        ssh: connect to host github.com port 22: Connection refused
        ```
        
    - **分析：** 连接 `github.com` 的 22 端口（SSH 默认端口）被拒绝。这通常表明本地或网络中的某个防火墙主动阻止了出站到 22 端口的连接。
    - **排查：** 针对 Windows 防火墙配置了出站规则，允许 TCP 协议的 22 端口连接。
4. **错误三：SSH 连接超时 (端口 22)**
    
    - **测试命令：** `ssh -T git@github.com`
    - **错误信息：**
        
        ```
        ssh: connect to host github.com port 22: Connection timed out
        ```
        
    - **分析：** 连接 22 端口超时，而非被拒绝。这更强烈地暗示问题出在**本地机器外部的网络环境**中，可能存在阻止 22 端口流量的防火墙或网络策略，导致连接请求无法到达或响应无法返回。
5. **解决方案：利用 GitHub 的备用 SSH 端口 (443)**
    
    - **思路：** 既然 22 端口被限制，而 443 端口（HTTPS 端口）通常是开放的，GitHub 也支持通过 443 端口进行 SSH 连接。
    - **操作：** 修改本地 SSH 配置文件 `~/.ssh/config`，添加配置使连接 `github.com` 时实际使用 443 端口和 `ssh.github.com` 主机名：
        
        代码段
        
        ```
        Host github.com
            Hostname ssh.github.com
            Port 443
            User git
        ```
        
    - **验证命令 (强制使用 443 端口):** `ssh -p 443 git@ssh.github.com`
6. **遇到认证提示 (正常步骤)**
    
    - **提示：**
        
        ```
        The authenticity of host '[ssh.github.com]:443 (...)' can't be established.
        ED25519 key fingerprint is SHA256:...
        Are you sure you want to continue connecting (yes/no/[fingerprint])?
        ```
        
    - **分析：** 第一次连接新主机或通过非标准端口连接时的安全提示，要求验证服务器身份（公钥指纹）。
    - **操作：** 比对终端显示的指纹与 GitHub 官方公布的指纹是否一致。确认无误后输入 `yes` 并回车，将主机信息添加到 `known_hosts` 文件。
7. **遇到私钥密码提示 (正常步骤)**
    
    - **提示：** `Enter passphrase for key 'C:\Users\11457/.ssh/id_rsa':`
    - **分析：** 要求输入生成 SSH 私钥时设置的密码，用于解密私钥以便进行认证。
    - **操作：** 输入正确的私钥密码（输入时不可见）。
8. **最终成功连接！**
    
    - **输出：**
        
        ```
        PTY allocation request failed on channel 0
        Hi moonify-J! You've successfully authenticated, but GitHub does not provide shell access.
        ```
        
    - **分析：** `PTY allocation failed` 是正常信息。`Hi moonify-J! You've successfully authenticated...` 明确表明使用 SSH 协议通过 443 端口，使用正确的 SSH Key 和密码，成功在 GitHub 上通过了身份认证。

**结论：**

最初的 HTTPS 和 SSH (端口 22) 连接失败很可能是因为所处的网络环境限制或防火墙阻止了标准的 443 和 22 端口。通过配置 SSH 客户端使用 GitHub 的备用 443 端口成功绕过了这个问题。现在可以使用 SSH 协议（例如 `git clone git@github.com/moonify-J/note.git`）进行 Git 操作了，Git 会根据 `~/.ssh/config` 配置自动使用 443 端口。

**关键要点：**

- 网络连接问题复杂，可能是本地防火墙或网络环境（公司、学校等）限制。
- "Connection refused" 和 "Connection timed out" 都指向网络阻断的可能性。
- SSH 协议是解决 HTTPS 连接问题的一种常用方法。
- 当 SSH 默认端口 22 被阻断时，尝试使用 GitHub 的备用 443 端口是一个有效的绕过方案。
- `~/.ssh/config` 文件是配置 SSH 客户端行为的重要工具。
- 首次 SSH 连接的主机指纹验证和私钥密码输入是正常的安全步骤。

---
### 防火墙设置出战规则

1. 打开“带有高级安全的 Windows Defender 防火墙”
2. 选择“出站规则”；创建新规则
3. 规则类型：端口
4. 指定协议和端口：TCP；特定远程端口
5. 允许连接
6. 指定配置文件：
	   在“配置文件”页面，选择此规则应用于哪种网络类型。通常为了确保在各种网络环境下都能使用 SSH，可以勾选 **“域(Domain)”、**“专用(Private)”** 和 **“公用(Public)”**。如果你只在家里（通常是专用网络）使用，至少确保“专用”被勾选。
7. 命名和描述规则
---
### 创建SSH密钥并使用ssh连接

1. 检查本地是否已存在 SSH Key
	```bash
	ls -al ~/.ssh
```
2. 如果没有则创建：
	```bash
	ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
	- `-t rsa`: 指定加密算法为 RSA。
    - `-b 4096`: 指定密钥长度为 4096 位（更安全）。
    - `-C "your_email@example.com"`: 添加注释，通常用你的邮箱地址，方便识别。请替换成你在 GitHub 上使用的邮箱。
	- 命令执行后，会提示你输入密钥保存的位置和文件名：
    
    ```
    Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):
    ```
    
    直接按回车使用默认位置和文件名 (`~/.ssh/id_rsa`) 即可。
- 接着会提示你输入一个密码 (passphrase)：
    
    ```
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    ```
    
    你可以选择输入一个密码，这样每次使用 SSH Key 时都需要输入密码，增加安全性。如果留空（直接按回车），则使用 SSH Key 时不需要输入密码，更方便（但安全性略低）。根据你的需要选择。
- 生成成功后，你会看到类似这样的输出：
    
    ```
    Your identification has been saved in /c/Users/you/.ssh/id_rsa.
    Your public key has been saved in /c/Users/you/.ssh/id_rsa.pub.
    The key fingerprint is: SHA256:... your_email@example.com The key's randomart image is: +---[RSA 4096]----+ | ... | +----[SHA256]-----+
    ```
3. 将公钥添加到 GitHub 账号
	- 复制公钥内容：
	```bash
	Get-Content ~/.ssh/id_rsa.pub
```
	- github账号添加ssh （略）
4. 测试SSH连接
	```bash
	ssh -T git@github.com
```
	- 第一次连接时，可能会出现一个提示，询问你是否确定要连接到 GitHub 的主机。输入 `yes` 并按回车。 ``` The authenticity of host 'github.com (IP地址)' can't be established. ECDSA key fingerprint is SHA256:... Are you sure you want to continue connecting (yes/no/[fingerprint])? yes  
		- 如果密钥显示正确（自行查看），则输入yes
		- 接着需要填写私钥密码，也就是创建ssh密钥时定义的密码
    
- 如果连接成功，你会看到类似这样的欢迎信息：
    
    ```
    Hi username! You've successfully authenticated, but GitHub does not provide shell access.
    ```
---
### 端口连接失败

**22端口连接失败时，先检查网络，再设置出战规则，都不成功，则使用GitHub备用端口443：**
1. 找到或创建 SSH 配置文件：
	```bash
	code ~/.ssh/config
```
2. 在打开的config文件中添加内容：
	```
	Host github.com
    Hostname ssh.github.com
    Port 443
    User git
	```
	- `Host github.com`: 这指定了该配置适用于主机名 `github.com`。
	- `Hostname ssh.github.com`: GitHub 的 SSH 服务也可以通过 `ssh.github.com` 这个别名访问。
	- `Port 443`: 关键设置，强制 SSH 客户端使用 443 端口连接。
	- `User git`: 指定连接时使用的用户是 `git`，这是 GitHub SSH 连接的标准用户。
3. 保存并关闭文件
4. 测试连接
	```bash
	ssh -T git@github.com
```
**或者：直接强制使用443端口**：
```bash
ssh -p 443 git@ssh.github.com
```

