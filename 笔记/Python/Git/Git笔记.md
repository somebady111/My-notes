# Git笔记

[TOC]



## 1.Git上传文件

```python
echo "# 1" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/somebady111/1.git
git push -u origin main

# git 调换当前传输位置
git remote add origin https://github.com/somebady111/1.git
git branch -M main
git push -u origin main

# 正常上传
git add *
git commit -m ''
git push -u origin master
```





## 2.Git下载文件

```python
git clone https://github.com/somebady111/1.git
```



