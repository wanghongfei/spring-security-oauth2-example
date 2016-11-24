# Spring Security OAuth2 Demo
项目使用的是MySql存储, 需要先创建以下表结构:
```

CREATE SCHEMA IF NOT EXISTS `alan-oauth` DEFAULT CHARACTER SET utf8 ;
USE `alan-oauth` ;

-- -----------------------------------------------------
-- Table `alan-oauth`.`clientdetails`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `alan-oauth`.`clientdetails` (
  `appId` VARCHAR(128) NOT NULL,
  `resourceIds` VARCHAR(256) NULL DEFAULT NULL,
  `appSecret` VARCHAR(256) NULL DEFAULT NULL,
  `scope` VARCHAR(256) NULL DEFAULT NULL,
  `grantTypes` VARCHAR(256) NULL DEFAULT NULL,
  `redirectUrl` VARCHAR(256) NULL DEFAULT NULL,
  `authorities` VARCHAR(256) NULL DEFAULT NULL,
  `access_token_validity` INT(11) NULL DEFAULT NULL,
  `refresh_token_validity` INT(11) NULL DEFAULT NULL,
  `additionalInformation` VARCHAR(4096) NULL DEFAULT NULL,
  `autoApproveScopes` VARCHAR(256) NULL DEFAULT NULL,
  PRIMARY KEY (`appId`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;


-- -----------------------------------------------------
-- Table `alan-oauth`.`oauth_access_token`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `alan-oauth`.`oauth_access_token` (
  `token_id` VARCHAR(256) NULL DEFAULT NULL,
  `token` BLOB NULL DEFAULT NULL,
  `authentication_id` VARCHAR(128) NOT NULL,
  `user_name` VARCHAR(256) NULL DEFAULT NULL,
  `client_id` VARCHAR(256) NULL DEFAULT NULL,
  `authentication` BLOB NULL DEFAULT NULL,
  `refresh_token` VARCHAR(256) NULL DEFAULT NULL,
  PRIMARY KEY (`authentication_id`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;


-- -----------------------------------------------------
-- Table `alan-oauth`.`oauth_approvals`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `alan-oauth`.`oauth_approvals` (
  `userId` VARCHAR(256) NULL DEFAULT NULL,
  `clientId` VARCHAR(256) NULL DEFAULT NULL,
  `scope` VARCHAR(256) NULL DEFAULT NULL,
  `status` VARCHAR(10) NULL DEFAULT NULL,
  `expiresAt` DATETIME NULL DEFAULT NULL,
  `lastModifiedAt` DATETIME NULL DEFAULT NULL)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;


-- -----------------------------------------------------
-- Table `alan-oauth`.`oauth_client_details`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `alan-oauth`.`oauth_client_details` (
  `client_id` VARCHAR(128) NOT NULL,
  `resource_ids` VARCHAR(256) NULL DEFAULT NULL,
  `client_secret` VARCHAR(256) NULL DEFAULT NULL,
  `scope` VARCHAR(256) NULL DEFAULT NULL,
  `authorized_grant_types` VARCHAR(256) NULL DEFAULT NULL,
  `web_server_redirect_uri` VARCHAR(256) NULL DEFAULT NULL,
  `authorities` VARCHAR(256) NULL DEFAULT NULL,
  `access_token_validity` INT(11) NULL DEFAULT NULL,
  `refresh_token_validity` INT(11) NULL DEFAULT NULL,
  `additional_information` VARCHAR(4096) NULL DEFAULT NULL,
  `autoapprove` VARCHAR(256) NULL DEFAULT NULL,
  PRIMARY KEY (`client_id`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;


-- -----------------------------------------------------
-- Table `alan-oauth`.`oauth_client_token`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `alan-oauth`.`oauth_client_token` (
  `token_id` VARCHAR(256) NULL DEFAULT NULL,
  `token` BLOB NULL DEFAULT NULL,
  `authentication_id` VARCHAR(128) NOT NULL,
  `user_name` VARCHAR(256) NULL DEFAULT NULL,
  `client_id` VARCHAR(256) NULL DEFAULT NULL,
  PRIMARY KEY (`authentication_id`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;


-- -----------------------------------------------------
-- Table `alan-oauth`.`oauth_code`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `alan-oauth`.`oauth_code` (
  `code` VARCHAR(256) NULL DEFAULT NULL,
  `authentication` BLOB NULL DEFAULT NULL)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;


-- -----------------------------------------------------
-- Table `alan-oauth`.`oauth_refresh_token`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `alan-oauth`.`oauth_refresh_token` (
  `token_id` VARCHAR(256) NULL DEFAULT NULL,
  `token` BLOB NULL DEFAULT NULL,
  `authentication` BLOB NULL DEFAULT NULL)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

```
然后在`oauth_client_details`表中插入记录:
```
# client_id, resource_ids, client_secret, scope, authorized_grant_types, web_server_redirect_uri, authorities, access_token_validity, refresh_token_validity, additional_information, autoapprove
'client', NULL, 'secret', 'app', 'authorization_code', 'http://www.baidu.com', NULL, NULL, NULL, NULL, NULL
```
这时就可以访问授权页面了:
```
localhost:8080/oauth/authorize?client_id=client&response_type=code&redirect_uri=http://www.baidu.com
```
访问时Spring让你登陆,随便输入一个用户名密码即可.

数据库连接信息在`application.properties`中配置。