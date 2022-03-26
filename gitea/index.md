# 如何使用gitea作为typora的图床？

# 如何使用gitea作为typora的图床？

1. 到PicGo官方github仓库的release下载对应平台的安装文件(选择有latest标签的）,并进行安装（这里以Windows 11为例，安装过程省略）

   [PicGo](https://github.com/Molunerfinn/picgo/releases)

2. typora配置PicGo的路径

   1. 打开typora软件，点击菜单栏的文件。

   ![image-20220326211755455](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326211755455.png)

   ​	2. 找到偏好设置（由于这个界面我截不了图所以这里就不放图了)，点击箭头所指的文件夹。

   ![image-20220326212023263](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326212023263.png)

    3. 选择picgo程序

       ![image-20220326212250031](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326212250031.png)

3. 打开PicGo，下载gitea插件。

   ![image-20220326212449573](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326212449573.png)

4. 创建gitea仓库，并完成第一次push

   ![image-20220326214020556](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326214020556.png)

   ![image-20220326214147688](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img//image-20220326214147688.png)

   ![image-20220326214559116](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326214559116.png)

   ```shell
   # 复制上面的代码，相应的调整一下
   mkdir img
   cd img
   mkdir img
   cd img
   touch README.md
   cd ..
   git init
   git add .
   git commit -m "first commit"
   git remote add origin https://example.com/你的用户名/img.git
   git push -u origin master
   ```

5. 配置gitea插件，找到图床设置，打开Gitea Img进行配置。

​	![image-20220326212652117](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326212652117.png)



```shell
URL			你gitea的域名，例如：https://example.com/
Usernaem	你gitea的用户名，例如：test
Repository	你gitea上用来存放图片的仓库名，例如：img
Token		参照下面的方法生成，然后复制粘贴上去
Path		仓库的相对路径，非填项，你也可以填，我就填img，你的仓库必须存在这个路径才可以填
```

![image-20220326213257477](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326213257477.png)

![image-20220326213206718](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326213206718.png)

![image-20220326213419078](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326213419078.png)

然后确认，也可以设为默认图床（建议设置）

![image-20220326213552467](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326213552467.png)

6. 最后验证一下配置。

   ![image-20220326215435459](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326215435459.png)

![image-20220326215703868](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326215703868.png)

出现这个配置就没有什么问题。

7. 最后配置一下上传规则就可以了，然后粘贴图片到typora就会自动上传的gitea了。

   ![image-20220326215842447](https://iuvhub.pro/hiifong/picgo/raw/branch/master/img/image-20220326215842447.png)

**enjoy！**


