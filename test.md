CREATE TABLE `user`(
    `id` long NOT NULL AUTO_INCREMENT primary key COMMENT '用户ID',
    `name` varchar(32) DEFAULT NULL COMMENT '用户姓名',
    `sex` ENUM('man', 'woman') NOT NULL COMMENT '性别',
    `phone` varchar(11) DEFAULT NULL COMMENT '电话号码',
    `email` varchar(30) DEFAULT NULL COMMENT '邮箱',
    `Sno` char(10) DEFAULT NULL COMMENT '学号'
)ENGINE = InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `competition`(
     `id` long NOT NULL AUTO_INCREMENT primary key COMMENT '比赛ID',
     `name` varchar(64) DEFAULT NULL COMMENT '比赛名字',
     `introduce` varchar(6000) NOT NULL COMMENT '比赛介绍',
     `stardate` DATETIME DEFAULT NULL COMMENT '比赛开始时间',
     `enddate` DATETIME DEFAULT NULL COMMENT '比赛结束时间'
     `奖项设计` DATETIME DEFAULT NULL COMMENT '奖项信息'
     `比赛附件` DATETIME DEFAULT NULL COMMENT '奖项信息'
)ENGINE = InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `通知`(
     `id` long NOT NULL AUTO_INCREMENT primary key COMMENT '通知ID',
)ENGINE = InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `team`(
    #`id` long NOT NULL AUTO_INCREMENT primary key COMMENT '队伍id',
    `captain_id` long DEFAULT NULL COMMENT '队长id',
    `user_id` long DEFAULT NULL COMMENT '队员id',
    `competition_id` long NOT NULL COMMENT '比赛id',
    primary key (captain_id,competition_id),
    FOREIGN KEY (captain_id) REFERENCES user (id),
    FOREIGN KEY (user_id) REFERENCES user (id),
    FOREIGN KEY (competition_id) REFERENCES competition (id)
)ENGINE = InnoDB DEFAULT CHARSET=utf8;