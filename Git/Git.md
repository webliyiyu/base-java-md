# Git

## 本地文件夹中下载远程仓库的 `movetake` 分支

1. **打开命令行界面** 并导航到该空文件夹。
2. **初始化一个新的Git仓库**：

```bash
git init
```

3. **添加远程仓库**。假设远程仓库的URL

```bash
git remote add origin url
```

4. **拉取远程的 `movetake` 分支**：

如果 `movetake` 分支已经在远程仓库中存在，你可以直接拉取并切换到这个分支：

```bash
git fetch origin movetake:movetake  
git checkout movetake
```

或者，你可以使用 `git checkout` 的 `-b` 选项来创建并切换到该分支（如果它在本地还不存在）：

```bash
git checkout -b movetake origin/movetake
```

1. **拉取该分支的最新更改**（如果需要）：

如果你之后还想拉取该分支的最新更改，你可以使用：

```bash
git pull origin movetake
```

## 将一个新的本地仓库里的内容添加到名为 `mytakeaway` 的远程分支下

1. **初始化 Git 仓库**（如果尚未初始化）：
   在你的本地项目文件夹中打开终端或命令行工具。如果还没有初始化为 Git 仓库，使用以下命令：

   ```bash
   git init
   ```

2. **添加所有文件到仓库**:
   使用以下命令将所有文件添加到 Git 跟踪中：

   ```bash
   git add .
   ```

3. **提交更改**:
   使用以下命令提交这些更改，并附上一条描述性信息：

   ```bash
   git commit -m "Initial commit"
   ```

4. **设置远程仓库**:
   如果你还没有设置远程仓库，需要设置一个。使用以下命令，其中 `<远程仓库URL>` 是远程仓库的 URL：

   ```bash
   git remote add origin <远程仓库URL>
   ```

5. **创建远程分支**:
   在推送前，确保远程仓库中有 `mytakeaway` 分支。如果该分支不存在，你将需要在推送时创建它。这可以通过使用 `-u` 参数和 `--set-upstream` 选项来实现：

   ```bash
   git push -u origin --set-upstream mytakeaway
   ```

   如果你已经设置了一个名为 `mytakeaway` 的远程分支，可以跳过这一步。

6. **推送到远程分支**:
   现在，你可以将你的代码推送到 `mytakeaway` 分支：

   ```bash
   git push origin mytakeaway
   ```

   如果这是第一次推送到这个远程分支，你可能需要使用 `-u` 参数来指定 `mytakeaway` 作为默认分支：

   ```bash
   git push -u origin mytakeaway
   ```

7. **检查远程分支内容**:
   为了验证你的代码已经成功推送到 `mytakeaway` 分支，你可以在远程仓库上进行检查，或者使用 `git branch -vv` 查看本地分支关联的远程分支。





## 配置常用命令的别名

```java
git config --global alias.<简写命令> <完整命令>
git config --global alias.gst status
git config --global alias.git-log "log --pretty=oneline --all --graph --abbrev-commit"
    
```



## 分支

![image-20240130181145286](../java/img/image-20240130181145286.png)

![image-20240129191502669](../java/img/image-20240129191502669.png)

![image-20240129191431405](../java/img/image-20240129191431405.png)

## 常用命令

```xml
1.初始化本地仓库
git init

2.工作区 》 暂存区	
git add .

3.暂存区 》 仓库 	
git commit -m '上传文件注释'

4.添加远程仓库
git remote add origin （gitee/github）URL

5.将本地仓库中的文件推送至远程仓库中
git push -u origin master


```

### 将远程仓库克隆到本地

```xml
git clone (gitee/github)URL 文件名
```

### 查看分支

```xml
git branch
```

### 创建切换分支

```xml
git checkout -b 分支名
```

### 合并分支

```xml
git merge
```

### 命令查看已配置的远程仓库及其对应的URL

```xml
git remote -v
```

### 要将名为 `origin` 的远程仓库的URL修改为 `https://github.com/user/repo.git`

```xml
git remote set-url origin https://gitee.com/the-yibai-daywind/hashmap.git
```

<<<<<<< Updated upstream
### 将master分支推送到远程仓库，并于远程仓库的master分支绑定关联关系

```xml
git push --set-uptream origin master
```

#### 查看当前工作树中未合并的文件列表

```xml	
git status
```

=======
=
>>>>>>> Stashed changes
