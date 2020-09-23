# Mysql

## 库结构

## 表结构
```shell

--查看表结构
SHOW FULL COLUMNS FROM act_seckill_shop_order 

-- 修改表名称
ALTER TABLE  auto200458_inviteLog  rename to auto200458_invitelog;


--查找相似表
select table_name,column_name from information_schema.columns where column_name like '%activity%';

```

## 字段结构
```shell


# 修改字段注释
ALTER TABLE act_seckill_shop_order MODIFY COLUMN expire_time datetime COMMENT '订单到期时间 30分钟';

# 删除字段
ALTER TABLE act_seckill_shop_order DROP pay_expire_time;

# 添加一个字段
ALTER TABLE auto200458_tasklog ADD shopId bigint(11) DEFAULT NULL COMMENT '汽车店Id';


```

## 其他
```shell

-- 添加唯一索引
alter table act_bb_user_statistics add unique key (`user_id`) ;
```
