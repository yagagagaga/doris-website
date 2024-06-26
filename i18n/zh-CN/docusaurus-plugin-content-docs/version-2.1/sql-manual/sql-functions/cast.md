---
{
    "title": "CAST",
    "language": "zh-CN"
}

---

<!-- 
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

## CAST

### Description

`CAST` 函数用于 SQL 查询中的数据类型转换。 它通常用于将一种数据类型转换为另一种数据类型，例如将字符串转换为整数、将整数转换为字符串等。

#### Syntax

`CAST (src_type as dst_type)`

将 src_type 转成指定的 dst_type 类型

### Example

1. 转常量，或表中某列

```mysql
mysql> select cast('1234' as int);
+---------------------+
| cast('1234' as INT) |
+---------------------+
|                1234 |
+---------------------+
```

2. 转导入的原始数据

```shell
curl --location-trusted -u root: -T ~/user_data/bigint -H "columns: tmp_k1, k1=cast(tmp_k1 as BIGINT)"  http://host:port/api/test/bigint/_stream_load
```

*注：在导入中，由于原始类型均为String，将值为浮点的原始数据做 cast的时候数据会被转换成 NULL ，比如 12.0 。Doris目前不会对原始数据做截断。*

如果想强制将这种类型的原始数据 cast to int 的话。请看下面写法：

```shell
curl --location-trusted -u root: -T ~/user_data/bigint -H "columns: tmp_k1, k1=cast(cast(tmp_k1 as DOUBLE) as BIGINT)"  http://host:port/api/test/bigint/_stream_load
```

```mysql
mysql> select cast(cast ("11.2" as double) as bigint);
+----------------------------------------+
| CAST(CAST('11.2' AS DOUBLE) AS BIGINT) |
+----------------------------------------+
|                                     11 |
+----------------------------------------+
1 row in set (0.00 sec)
```

对于DECIMALV3类型，`CAST` 会进行四舍五入：

```mysql
mysql> select cast (1.115 as DECIMALV3(16, 2));
+---------------------------------+
| cast(1.115 as DECIMALV3(16, 2)) |
+---------------------------------+
|                            1.12 |
+---------------------------------+
```

### Keywords

CAST