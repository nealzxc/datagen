datagen
====

excel向json\lua\php文件转换，使用excel进行产品配置，导出为json\lua\php格式的配置文件。

## 支持数据类型
+ i    : 整数
+ f    : 浮点数
+ b    : 布尔
+ s    : 字符串
+ {}   : 对象
+ []   : 数组
+ [{}] : 对象数组

## 规则
1: sheet分为三类：master, sub, meta
   + meta为配置信息条，一个文档中只能有一个，用以配置输出文件名，输出格式等。
   + master为主数据表，一个文档中可以有多个，一个master可以对应多个sub sheet作为外链，存在多个master表则对应多个输出文件。
   + sub为外链表，一个文档中可以有多个，一个sub sheet可以对应多个sub sheet作为外链
   + 使用外链时候，sheet名称为@之前的部分字符串。

2: 表头规则
   + 基本规则：[表头名]#格式，如id#id, name#s, level@, skill#{}, other$[{}], upgrade@
   + 外链规则：[表头名]@, 如upgrade@，此列数据以upgrade@hero做为数据源
   + 如果一个master sheet中含有#id标记的表头，则作为对象输出，如果没有#id标记的表头，则作为数组输出，
   + 如果当前sheet只有一条数据时，可以在meta中设置single_obj强制输出为对象。

3: 数据项规则
	+ 数组成员之间使用`,`分割，如：s10101,s10103
	+ 对象成员之间使用`;`分割，对象内部采用`:`分割，如：11:1;12:2;13:3;14:4;15:5
	+ 对象数组时,数组成员间采用`,`分割，对象间采用`;`分割,如：id:2;level:30,id:3;level:80

4: 外链类型
   sheet名称规则为列名@父sheet名称，如当前sheet名称为hero，可以使用一个名称为level@hero的sheet为hero添加一个level的属性.
   如再为level添加一个属性skill，可以使用skill@level的sheet为level添加属性。通过此种外链，实现对复杂数据结构的支持。
