Table Engine
==

InnoDB
--

``` text
1. 具有提交、回滚和崩溃恢复能力的事物安全（ACID兼容）存储引擎, 行级锁, 非锁定读  
2. 为处理巨大数据量的最大性能设计, 它的CPU效率可能是任何其他基于磁盘的关系型数据库引擎锁不能匹敌的  
3. 完全与MySQL服务器整合, InnoDB存储引擎为在主内存中缓存数据和索引而维持它自己的缓冲池  
4. 支持外键完整性约束  
```

MyISAM
--

``` text
静态MyISAM: 如果数据表中的各数据列的长度都是预先固定好的, 服务器将自动选择这种表  
           类型。因为数据表中每一条记录所占用的空间都是一样的, 所以这种表存取和  
           更新的效率非常高。当数据受损时, 恢复工作也比较容易做。  
动态MyISAM: 如果数据表中出现varchar、xxxtext或xxxBLOB字段时, 服务器将自动选择这  
           种表类型。相对于静态MyISAM, 这种表存储空间比较小, 但由于每条记录的长  
           度不一, 所以多次修改数据后, 数据表中的数据就可能离散的存储在内存中,   
           进而导致执行效率下降。同时, 内存中也可能会出现很多碎片。因此, 这种类  
           型的表要经常用optimize table 命令或优化工具来进行碎片整理。
压缩MyISAM: 以上说到的两种类型的表都可以用myisamchk工具压缩。这种类型的表进一步  
           减小了占用的存储, 但是这种表压缩之后不能再被修改。另外, 因为是压缩数  
           据, 所以这种表在读取的时候要先时行解压缩。  

所有MyISAM都不支持事务, 行级锁和外键约束的功能。
```

MyISAM Merge
--

    这种类型是MyISAM类型的一种变种。合并表是将几个相同的MyISAM表合并为一个虚表。常应用于日志和数据仓库。

memory(heap)
--

    只存在于内存中, 使用散列索引, 常应用于临时表

archive
--

    只支持select 和 insert语句, 而且不支持索引, 应用于日志记录和聚合分析方面

InnoDB VS MyISAM
--

``` text
1. 索引文件结构  
   InnoDB的数据文件本身就是索引文件, 索引树的叶节点data域保存了完整的数据记录  
   MyISAM索引文件和数据文件是分离的  
2. 辅助索引data域存储相应记录  
   InnoDB存储相应记录主键的值(自增字段作为主键是一个很好的选择)  
        因为所有辅助索引都引用主索引, 过长的主索引会令辅助索引变得过大  
   MyISAM存储相应记录主键的地址  

3. 事务支持  
   InnoDB - YES  
   MyISAM - NO  
4. 锁级别  
   InnoDB - 行锁, 乐观锁, 读共享锁  
   MyISAM - 表锁  
5. 外键支持  
   InnoDB - YES  
   MyISAM - NO  

6. 用途区别  
   InnoDB - 数据关联性强的应用场景  
   MyISAM - 主要用来插入和查询并且不要求事务  

只有最合适的解决方案, 没有最好的解决方案
```