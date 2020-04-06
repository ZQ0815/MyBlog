CREATE TABLE `user`(
    `id` long NOT NULL AUTO_INCREMENT primary key COMMENT '用户ID',
    `name` varchar(32) NOT NULL COMMENT '用户姓名',
    `sex` enum('0','1') NOT NULL DEFAULT '0' COMMENT '性别：0-男，1-女',
    `phone` varchar(11) NOT NULL COMMENT '电话号码',
    `email` varchar(30) NOT NULL COMMENT '邮箱',
    `Sno` char(10) DEFAULT NULL COMMENT '学号',
    `create_time` DATETIME NOT NULL COMMENT '创建时间',
    `update_time` DATETIME NOT NULL COMMENT '更新时间'
)ENGINE = InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `competition`(
     `id` long NOT NULL AUTO_INCREMENT primary key COMMENT '比赛ID',
     `name` varchar(64) DEFAULT NULL COMMENT '比赛名字',
     `introduction` varchar(3000) NOT NULL COMMENT '比赛介绍',
     `star_date` DATETIME NOT NULL COMMENT '比赛开始时间',
     `end_date` DATETIME NOT NULL COMMENT '比赛结束时间',
     `create_time` DATETIME NOT NULL COMMENT '创建时间',
     `update_time` DATETIME NOT NULL COMMENT '更新时间'
)ENGINE = InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `competition_announcement`(
      `id` long NOT NULL AUTO_INCREMENT primary key COMMENT '公告id',
      `content` varchar(3000) NOT NULL COMMENT '公告内容',
      `competition_id` long NOT NULL COMMENT '比赛id',
      `create_time` DATETIME NOT NULL COMMENT '创建时间'
)ENGINE = InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE `team`(
    `id` long NOT NULL AUTO_INCREMENT primary key COMMENT '队伍id',
    `name` varchar(30) NOT NULL COMMENT '队伍名称',
    `competition_id` long NOT NULL COMMENT '比赛id',
    `captain_id` long NOT NULL COMMENT '队长id',
    `create_time` DATETIME NOT NULL COMMENT '创建时间',
    `update_time` DATETIME NOT NULL COMMENT '更新时间'
)ENGINE = InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `team_member`(
    `team_id` long NOT NULL COMMENT '队伍id',
    `user_id` long NOT NULL COMMENT '队员id',
    `member_type` enum('0','1') NOT NULL COMMENT '队员类型：0-队长，1-队员',
    `create_time` DATETIME NOT NULL COMMENT '创建时间',
    `update_time` DATETIME NOT NULL COMMENT '更新时间'
)ENGINE = InnoDB DEFAULT CHARSET=utf8;

# 常用查询条件
# 如何建立索引
# 检查是否有字段冗余、范式的级别
# 插入数据，进行增删查改性能分析