# 中华人民共和国行政区划代码
## GB/T 2260-2007
《中华人民共和国行政区划代码》（GB/T 2260-2007）是2008年2月1日开始实施的一项中华人民共和国国家推荐性标准，归口于全国信息分类与编码标准化技术委员会。 [百度百科](https://baike.baidu.com/item/%E4%B8%AD%E5%8D%8E%E4%BA%BA%E6%B0%91%E5%85%B1%E5%92%8C%E5%9B%BD%E8%A1%8C%E6%94%BF%E5%8C%BA%E5%88%92%E4%BB%A3%E7%A0%81/56773615?fr=aladdin)
[国家标准全文公开系统](https://openstd.samr.gov.cn/bzgk/gb/newGbInfo?hcno=C9C488FD717AFDCD52157F41C3302C6D) [政府文件]
## 适用数据库

- mysql

### 数据库导入优化

- 批量导入方式, 每批次包含适量的条数. 导入速度更快
 
### 文件说明

- [cnarea_2023.tar.gz](./cnarea_2023.tar.gz) 五级行政区包括港澳台数据
- [cnarea_simples.tar.gz](cnarea_simples.tar.gz) 三级行政区数据, 不包含港澳台数据
### 下载, 解压
```
git clone https://github.com/hhniao/cnarea
cd cnarea
tar -zxvf cnarea_2023.tar.gz
tar -zxvf cnarea_simples.tar.gz
```
### 导入方式1
```
mysql -u<username> -p<password>
set names utf8mb4; // 设置编码
use test           //选择数据库
source ./cnarea_2023.sql
source ./cnarea_simples.sql
```
### 导入方式2 
```
> mysql -h127.0.0.1 -uroot -proot -D your_db  < ./cnarea_2023.sql
```

## 中国5级行政区域mysql库

爬取国家统计局官网的行政区域数据,包括省市县镇村5个层级;

港澳地区的数据只有3级;台湾地区4级;

包含大陆地区的邮政编码和经纬度信息.
  
---------------------------------------
全部共 **758049** 条

- 大陆数据共**679237** 条,其中
    - 省/直辖市 `31`
    - 市/州 `342`
    - 县/区 `3348`
    - 乡/镇 `42757`
    - 村/社区 `632759`

- 港澳台数据共**78812** 条,其中
    - 省/特区 `3`
    - 港澳辖区 `33`
    - 台湾市/县 `23`
    - 台湾区/镇 `371`
    - 台湾街道/村 `78384`

### 表结构

```mysql

CREATE TABLE `cnarea_2023`
(
    `id`          mediumint(7) unsigned          NOT NULL AUTO_INCREMENT,
    `level`       tinyint(1) unsigned            NOT NULL COMMENT '层级',
    `parent_code` bigint(14) unsigned            NOT NULL DEFAULT '0' COMMENT '父级行政代码',
    `area_code`   bigint(14) unsigned            NOT NULL DEFAULT '0' COMMENT '行政代码',
    `zip_code`    mediumint(6) unsigned zerofill NOT NULL DEFAULT '000000' COMMENT '邮政编码',
    `city_code`   char(6)                        NOT NULL DEFAULT '' COMMENT '区号',
    `name`        varchar(50)                    NOT NULL DEFAULT '' COMMENT '名称',
    `short_name`  varchar(50)                    NOT NULL DEFAULT '' COMMENT '简称',
    `merger_name` varchar(50)                    NOT NULL DEFAULT '' COMMENT '组合名',
    `pinyin`      varchar(30)                    NOT NULL DEFAULT '' COMMENT '拼音',
    `lng`         decimal(10, 6)                 NOT NULL DEFAULT '0.000000' COMMENT '经度',
    `lat`         decimal(10, 6)                 NOT NULL DEFAULT '0.000000' COMMENT '纬度',
    PRIMARY KEY (`id`),
    UNIQUE KEY `uk_code` (`area_code`) USING BTREE,
    KEY `idx_parent_code` (`parent_code`) USING BTREE
) ENGINE = MyISAM
  DEFAULT CHARSET = utf8 COMMENT ='中国行政地区表';
```
### 数据来源
[引用](https://github.com/kakuilan/china_area_mysql)
