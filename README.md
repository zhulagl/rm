# rm

## 小技巧

### 无扩展名的文件 默认打开方式
- [ ] 管理员 打开cmd 输入命令
- assoc .="No Extension"
- ftype "No Extension"="C:\Program Files\Microsoft VS Code\Code.exe" "%1"
> vscode编辑器的路径
### SConscript 修改编译包含文件目录
```
list  = [cwd + '/mypkgs']
list += [cwd + '/board']
list += [cwd + '/libraries']
list += [cwd + '/rt-thread']
list += [cwd + '/applications']
```
### Keil 选项
> 下载器时钟速率过高,可能造成错误,将clock改为1MHz比较稳定
> 注意下载线的连接可靠性,可特制下载插口连接器,提高稳定性.
### git问题之 子.git无法push成功
> 问题现象: 当git仓库工程目录下, 包含其他.git仓库时, git冲突. 没有深入研究冲突内容.
> 而rtt的工程中,使用.gitignore将包含.git仓库的packages文件夹忽略舍去了.
> 从这里看出,子git仓库是不被打算加入工程目录作为版本控制和push pull操作的.
> 问题原因: 要规避子.git

解决办法: 避开.git有几个办法,压缩文件夹上传,下载后解压,但这样频繁手动管理,不容易实现.
新建文件夹,作为影子包,其中拷贝所有packages文件数据,然后删除各个.git仓库管理文件夹.
这样push 和 pull 可以完整保留影子包,而packages被git忽略,同时不影响env和工程构建.
此做法有可行性.


- 新建影子包:mypkgs
- 编译包含影子包目录,那么修改SConscript,使包含影子包,不包含packages包

若不小心添加了 子.git
```
$ git add .
warning: adding embedded git repository: h743/mypkgs/agile_led-latest
hint: You've added another git repository inside your current repository.
hint: Clones of the outer repository will not contain the contents of
hint: the embedded repository and will not know how to obtain it.
hint: If you meant to add a submodule, use:
hint:
hint:   git submodule add <url> h743/mypkgs/agile_led-latest
hint:
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint:
hint:   git rm --cached h743/mypkgs/agile_led-latest
hint:
hint: See "git help submodule" for more information.
```
可以看出,方法有两个 : submodule 和 rm --cached
当看到这个warning后, 继续commit push, 会发现云上的软件包是空文件夹,
即git未知obtain. not contain the contents of the embedded repository
我们必须重新push完整的文件目录.
> 需要做的是
> git rm --cached ////
> 这把git仓库中缓存的软件包目录移除了.
我们手动修理好.git 然后重新 git add commit push 就可以正常上传了.
之所以不选择submodule 是因为 push的时候也会子.git push.这冲突了权限问题.

### keil 与 vscode

- vscode 安装 keil assistant 插件后, 可以直接打开 UV工程
- 设置c_cpp_properties.json 中 "intelliSenseMode": "clang-arm",消除 "_WIN32" 编译预定义.
- vscode 可以菜单操作 git add / commit / push, 但是在vscode软件之外操作的要注意特殊处理





