# 建站遇到的问题总结


想到哪写到哪。

## 参考资料

[官方文档](https://gohugo.io/documentation/)

[LoveIt主题样例网站](https://hugoloveit.com/zh-cn/)

[Hugo 从入门到会用](https://olowolo.com/post/hugo-quick-start/)


## 搭建时遇到的错误和Tips

### GitHub Pages 404

解决：repository必须要设定成公开。（Hexo似乎可以是私人所以一开始都没注意到这个问题）

虽然本来就对隐私问题比较过敏，设定成公开之后又会觉得被fork了怎么办，但是其实打算使用公开可被搜索到的博客就已经不需要考虑这些事了，被爬也是一样的嘛。

### GitHub 同一台电脑多账号

此前使用的Hexo blog建在了工作用账号的GitHub Pages上，所以才一直都不想用。本次下定决心使用博客所以创建了第二个账号，也许将来会有勇气和主号合并，但未来的事现在就不要去焦虑了。

简单的创建步骤：

1. 新建账号
2. 请求新的ssh key

```bash
ssh-keygen -t rsa -C "email_1" -f ~/.ssh/id_rsa_user1
ssh-keygen -t rsa -C "email_2" -f ~/.ssh/id_rsa_user2
```

3. 设置config文件。在`~/.ssh`文件夹中创建`config`文件：

```bash
# 账号1
Host github.com-user1
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_user1

# 账号2
Host github.com-user2
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_user2
```

对任一账号的仓库：

```bash
git@github.com-user1:username1/repository.git
```

4. （似乎有必要的步骤，忘了）在ssh-agent添加两个账号。似乎无法在git gui调用，需要使用cmd

```bash
ssh-add ~/.ssh/id_rsa_user1
ssh-add ~/.ssh/id_rsa_user2
```

可使用 `ssh-add -l` 确认是否添加成功。

5. 设定与特定的本地仓库交互时使用的默认账户，在本地仓库的根目录下设置：

```bash
git config user.name
git config user.email
```

### GitHub Pages deploy error

遇到了编译错误：

```
github-pages 228 | Error:  Liquid syntax error (line 4): Variable '{{"<"}} style "img { height: 1.25rem; }" {{">"}}' was not properly terminated with regexp: /\}\}/
```

应该是主题模板的问题（本博客目前由LoveIt的样例网站修改而成，并非从0自己搭建~~我是真的恨web~~），涉及到Hugo的shortcode，但被GitHub用Jekyll识别，具体没有深入研究~~我是真的恨~~。

解决：简单地在本地仓库添加名为`.nojekyll`、内容空白的文件。

进一步在上述报错代码中遇到错误：因为包含了shortcode的大括号，解决方法如上，使用了`{{"<"}} `避免识别。
