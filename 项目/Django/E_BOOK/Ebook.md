# 项目：Ebook

**APP**

## 图书app（BOOK）

`1.图书信息表(Book_Info)`

id  autofield(primary_key=True)

book_name  CharField('图书名称')

book_author CharField('图书作者')

book_info CharField('图书信息')

book_content TextField('图书内容')

book_chapter CharField(图书章节)

is_navigation BoolField(是否导航)

creat_book_time DateTimeFiled('入库时间')

- [x] book_status Boolfield(图书状态) 


book_link URLField(图书链接）



## 评论app（comment）

`1.评论表Book_comment`

id(primary_key=True)

comment_user ForeignKey()

comment_content

creat_comment_time



## 用户app（USE）

`1.用户信息表Users`

id = AutoField(primary_key=True)

user_nickname CharField()

user_password Int

user_info CharField()

creat_user_time DateTimeField()

user_status PositiveIntegerField()

is_navigation BoolField()

user_login_time DateTimeField()



`2.阅读记录表ReadNotes`

id AutoField(primary_key=True)

read_book_name ForeignKey()

read_book_time DateTimeField()

is_navigation BoolField()



`3.书签记录表BookmarkRecord`

id AutoField(primary_key=True()

write_book_content CharField()

write_note_time DateTimeField()

note_status PositiveIntegerField()

is_navigation BoolField()

​	

##### 备注：

1.用户能够作什么？a.登录b.查看自己信息c.修改自己信息d.看书（用户登录后才可以拥有的权限）e.查看阅读历史记录（1对1关系）f.

2.每个用户进入界面登陆后，看到的只是自己的界面。

3.图书和评论的关系：一本书可以多人评论，每个人可以看到当前刷新后的所有评论





###### 问题：

1.模块路径引导错误   解决

2.图书状态book_status类型错误

3.



