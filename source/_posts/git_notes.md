title: git 基本命令
date: 2016-11-18 17:14:32
categories: git
tags: [git clone],[git tag]
description: git

---
git clone git tag 基本操作以及gitignore

<!--more-->
### Git Clone 项目基本命令


######Git global setup
	git config --global user.name "IAskWind"
	git config --global user.email "iaskwind@foxmail.com"
	
######Create a new repository	
	git clone https://github.com/IAskWind/IAWExtensionTool.git
	cd IAWExtensionTool
	touch README.md
	git add README.md
	git commit -m "add README"
	git push -u origin master

######Existing folder or Git repository
	cd existing_folder
	git init
	git remote add origin https://github.com/IAskWind/IAWExtensionTool.git
	git add .
	git commit
	git push -u origin master		
		
### Git tag 基本命令
	git push origin --delete tag '0.1.1' 删除远程tag
	git tag 查看本地tag
	git tag '0.1.2' 本地创建tag
	git push --tags 本地tag 提交到服务器
	
### .gitignore
	如果文件已经提交到版本库里了，然后再把它加到忽略文件里，需要删掉文件，
	然后git rm 删掉本地库，然后add pull push 到服务器即可
	
### 开源让世界更美好   
### 感谢
