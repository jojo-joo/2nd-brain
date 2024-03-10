## Install Git and Setup
Open VScode, click on the source control management interface, then click "Download Git for Windows" to open the download link.

![alt text](<assets/Managing projects through VScode and GitHub/image-1.png>)

> **NOTE:**
> 
> During the installation process, simply click "Next" continuously.
>
> Use the option `Use Visual Studio Code as Git's default editor`

Open `cmd` and input `git`, show following if normal.

![alt text](<assets/Managing projects through VScode and GitHub/image-4.png>)

``` shell
git config --global user.name "your_user_name"
git config --global user.email "your_email"
git config --global -l
```

## Push

Open Folder by VSCode

![alt text](<assets/Managing projects through VScode and GitHub/image-3.png>)

Click on "Initialize Repository".

![alt text](<assets/Managing projects through VScode and GitHub/image-5.png>)

You can see that the project is created. Press "Commit"

![alt text](<assets/Managing projects through VScode and GitHub/image-6.png>)

A dialog popup, Click "Save All & Commit Changes"

![alt text](<assets/Managing projects through VScode and GitHub/image-8.png>)

Beware of the bottom right corner, there is a notification of null commit messages

![alt text](<assets/Managing projects through VScode and GitHub/image-9.png>)

Add a message of "first commit", and "Commit" Again

![alt text](<assets/Managing projects through VScode and GitHub/image-10.png>)

Click "Push"

![alt text](<assets/Managing projects through VScode and GitHub/image-7.png>)


选择从GitHub添加远程存储库



登录GitHub并授权与VScode进行连接







打开GitHub ，创建一个test库



这里会显示如何使用git命令如何推送，我们这里使用的是VScode来推送的



回到VScode进行推送，点击确定



回到GitHub刷新页面，就可以看到main分支推送消息推送一个测试文件



 可以在VScode创建一个新的分支first



 可以看到左下角的main分支变成了first分支，再次进行推送



可以看到GitHub上的分支多出了一个first分支，



我们将Home.vue的文件的其他按钮删除，只留一个默认按钮，可以看到会记录到更改的文件





点击暂存所有更改，点击左下角的first分支，然后选择推送到main分支





左下角变成main分支，再进行推送


 到GitHub刷新一下，可以看到main分支的推送消息只留了默认按钮，同时Home.vue的代码只留下了默认按钮，而first分支的Home.vue则还保留着其他的





 三、拉取
 需要用到其他分支或者下载其他项目资源，就可以通过拉取的操作下载下来，这里以test库的first分支为例，新建一个first空文件夹，然后用VScode打开，打开终端，输入git clone 项目地址，就可以拉取到

首先到GitHub上复制地址，这里选择的是HTTPS的地址



回到VScode进行下载

git clone https://github.com/zhangHaoQiang824/test.git


下载完成后点开Home.vue查看是否是first分支的，可以看到已经拉取到了first分支的test文件

