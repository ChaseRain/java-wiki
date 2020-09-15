# Mysql
```shell
ALTER TABLE auto200458_invite CHANGE column shopId inviteShopId bigint(11) comment '邀请人的汽车店Id';


-- 删除一个字段
ALTER TABLE  auto200458_tasklog DROP COLUMN shopId;


-- 添加一个字段
ALTER TABLE auto200458_tasklog ADD shopId bigint(11) DEFAULT NULL COMMENT '汽车店Id';

-- 修改表名称
ALTER TABLE  auto200458_inviteLog  rename to auto200458_invitelog;

-- 添加唯一索引
alter table act_bb_user_statistics add unique key (`user_id`) ;



--查找相似表
select table_name,column_name from information_schema.columns where column_name like '%activity%';

--表字段
show full columns from act_bb_boost_invite



```
