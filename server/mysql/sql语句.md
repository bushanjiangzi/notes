## 查询所有表

-- show tables;

## 新增

-- insert into users(username,`password`,realname)values('zhangsan','123456','张三');

## 查询

-- select _ from users;
-- select id,username from users;
-- select _ from users where username='lisi';
-- select _ from users where username='lisi'and`password`='123456';
-- select _ from users where username='lisi'or`password`='123456';
-- select _ from users where username like '%li%'
-- select _ from users order by id desc

## 更新

-- update users set realname='李四 2' where username='lisi'

## 删除

-- delete from users where username='zhangsan'

## 软删除

-- 通过设置 state 实现软删除(不等于 0：<>'0')
-- select \* from users where state='1';
-- update users set state='1' where username='lisi'

-- blogs 表
-- insert into blogs(title,createtime,content,author)values('java 开发从入门到放弃','1588588461360','很久以前有一个人叫李四，有一天。。。','李四');
-- select \* from blogs where author='李四' order by createtime desc
