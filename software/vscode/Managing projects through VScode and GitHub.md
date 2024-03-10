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

## Commit to Local Repository

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

Everything is commited, and you will see

![alt text](<assets/Managing projects through VScode and GitHub/image-11.png>)

So far, everything is stored in your local computer, next step we push it to the remote repository.

## Push to remote repository

Click "Push"

![alt text](<assets/Managing projects through VScode and GitHub/image-7.png>)

There is a notification in the bottom right corner.

![alt text](<assets/Managing projects through VScode and GitHub/image-12.png>)

Click "Add Remote"

![alt text](<assets/Managing projects through VScode and GitHub/image-13.png>)

There is a popup on the top center, click "Add remote from GitHub"

![alt text](<assets/Managing projects through VScode and GitHub/image-14.png>)

- Click "Allow", agree and enter your password.
- Login GitHub, create a repository with the same name of your folder
- Click "Push" again

![alt text](<assets/Managing projects through VScode and GitHub/image-16.png>)

popup information refreshed this time, choose the git url, everythin will be sent to remote repository. 
Refresh the web page of GitHub, you will see the updated files.

> ERROR
> 
> if shown error like "Failed to connect to github.com port 443 after 29006 ms: Couldn't connect to server
>
> Please use a proxy
```shell
git config --global http.proxy 127.0.0.1:7890
git config --global https.proxy 127.0.0.1:7890
```

