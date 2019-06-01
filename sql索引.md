# **索引**

作用：提高查询效率

定义：索引是关系数据库中对某一列或多列的值进行预排列的数据结构。

例如，students表：

| id   | class_id | name | gender | score |
| ---- | -------- | ---- | ------ | ----- |
| 1    | 1        | 小明 | M      | 90    |
| 2    | 1        | 小红 | F      | 95    |
| 3    | 1        | 小军 | M      | 88    |

如果经常对score列进行查询，就可以对score列创建索引：

**ALTER TABLE** students **ADD** INDEX  idx_score(score);

如果索引有多列，可以在括号写上，例如：

**ALTER TABLE** students **ADD** INDEX idx_name_score(name,score);

注：索引列的值越不相同，索引效率就越高

**缺点**：在插入、更新和删除记录时，需要同时修改索引，因此，索引越多，插入、更新和删除记录的速度就越慢。

对于主键，关系数据库会自动对其创建主键索引。使用主键索引的效率是最高的，因为主键会保证绝对唯一。

### 唯一索引

在设计关系数据表的时候，看上去唯一的列，例如身份证号、邮箱地址等，因为他们具有业务含义，因此不宜作为主键。

但是，这些列根据业务要求，又具有唯一性约束：即不能出现两条记录存储了同一个身份证号。这个时候，就可以给该列添加一个唯一索引。例如，我们假设`students`表的`name`不能重复：

**ALTER TABLE** students **ADD UNIQUE** INDEX  uni_name(name);

通过unique关键字就添加了一个唯一索引。

还可以只对某一列添加一个唯一约束而不添加唯一索引：

**ALTER TABLE** students **ADD CONSTRAINT** uni_name **UNIQUE** (name);

这种情况下，`name`列没有索引，但仍然具有唯一性保证。