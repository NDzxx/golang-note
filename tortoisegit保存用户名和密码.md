#win下TortoiseGit 连接oschina不用每次输入用户名和密码的方法

每次git clone 和push 都要输入用户名和密码。虽然安全，但在本机上每次都输有些麻烦，如何记住用户名和密码呢？
在网上看了各种方法，太杂，很多可能环境不一样，一直行不通。最后找到一种有效的方法，很简单。记录下来！

当你配置好git后，在C:\Documents and Settings\Administrator\ 目录下有一个 .gitconfig 的文件，里面会有你先前配好的name 和email，只需在下面加一行

```git
[credential]
 helper = store
```

下次再输入用户名 和密码 时，git就会记住，从而在C:\Documents and Settings\Administrator\ 目录下形成一个 .git-credentials 文件，里面就是保存的你的用户名和密码。 

