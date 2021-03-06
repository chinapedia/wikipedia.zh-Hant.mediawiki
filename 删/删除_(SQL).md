{{NoteTA
|G1=IT
}}
在[[SQL|SQL]]裡，'''DELETE'''语句用于从表中删除一个或多个数据。使用它需要定义一个子集作为条件，否则表中的所有数据都会被删除。

==用法==
<code>DELETE</code> 语句的一半语法为:

:'''<code>DELETE</code>''' <code>FROM</code> ''表名'' ['''<code>WHERE</code>''' 条件]

该语句能够使表中所有满足[[Where_(SQL)|<code>WHERE</code>]]子句条件的[[元组|元组]]都会被删除。如果缺少 <code>WHERE</code> 子句，则表中所有的元组都会被删除。

执行一条 <code>DELETE</code> 语法能够触发[[数据库触发器|触发器]]而在其他表中执行删除操作。例如，有[[外码|外码]]相联系的两个表，如果作为被参照关系的表中的元组被删除，则作为参照关系的表也会被删除，以保证关系的[[参照完整性|参照完整性]]。

==示例==
*从表 ''pies''中删除''flavour''为''Lemon Meringue''的元组:
<source lang="sql">
DELETE FROM pies WHERE flavour='Lemon Meringue';
</source>

*从表''trees''中删除''height''低于80的元组.
<source lang="sql">
DELETE FROM trees WHERE height < 80;
</source>

*删除表''mytable''中所有的元组:
<source lang="sql">
DELETE FROM mytable;
</source>
*删除表 ''mytable''中符合子查询结果的元组:
<source lang="sql">
DELETE FROM mytable WHERE id IN (SELECT id FROM mytable2)
</source>

==参考==
*{{Translation/Ref|lang=en|article=Delete (SQL)|oldid=}}
*{{cite book zh |author=王珊 萨师煊 |title=数据库系统概论 |format=M |edition=4 |location=北京 |publisher=高等教育版社 |date=2006 |pages= |id= ISBN 7-04-019583-6}}

{{SQL}}
{{Databases}}
{{Compu-lang-stub}}

[[Category:SQL关键字|Category:SQL关键字]]