# 🔘Menu

```button
name 创建文章
type command
action QuickAdd: Run QuickAdd
color blue
```
# 📚文章列表
```dataview
Table without id
	link(file.link, title) as 文章标题,
	categories as 分类,
	tags as 标签,
	date as 发布时间
from "content/post" and !"content/post/_index"
sort file.ctime desc

```
