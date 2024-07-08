# Github+PicGo+Typora构建图床

一、创建Github仓库
![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/2.png)

二、点击头像，选择Settings

![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/3.png)

三、选择左侧栏**Developer Settings**

四、选择Tokens
![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/4.png)

五、生成新token

![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/5.png)

六、填个Note (随便填)，只勾选repo即可，点击最下面Generate token
![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/6.png)

七、保存好token，只显示一次
![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/7.png)

八、Github下载开源的PicGo

![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/8.png)

九、配置PicGo的GitHub设置

![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/20240411200518.png)

* 图床配置名: 随意
* 仓库名为: 用户名/仓库名
* 分支: 你的分支
* token: 刚才生成的token
* 存储路径: 上传仓库中图片目录
  * 设置路径为 pictures，地址返回为
  * `https://raw.githubusercontent.com/${your_github_account}/Images/main/pictures/xxx.png`
* 自定义域名: `https://cdn.jsdelivr.net/gh/GitHub账号名/仓库名` 可以配置cdn
* `https://cdn.jsdelivr.net/gh/`是 jsDelivr 的 CDN（内容分发网络）服务地址，可以用于托管 GitHub 仓库中的文件和资源。通过这个地址，你可以直接访问 GitHub 仓库中的文件，而无需使用 GitHub 网站或者 Git 工具

> PicGo文档`https://picgo.github.io/PicGo-Doc/`

十、配置Typora自动自动上传

* 点击文件==>偏好设置

* ![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/20240411203136.png)

* 正常从本地引入图片,引入后出现下拉列表，点击上传图片，可自动上传
* ![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/20240411203856.png)

