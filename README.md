# 中华人民共和国行政区划代码
## GB/T 2260-2007
## mysql

### 快速导入一分钟导入所有数据
### 数据库导入优化

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
