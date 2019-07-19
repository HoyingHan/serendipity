## 【MySQL】列转行



### 需求

![](D:\code\MarkdownPhotosRepository\Res\ArticlePic\mysql列转行1.png)

需要根据key不同，添加key类型相应的列



`SELECT 
item_id,
item_name,
MAX(case 
	WHEN `key` = 'home' 
	THEN pv END) as home_pv,
MAX(case 
	WHEN `key` = 'home' 
	THEN uv END) as home_uv,
MAX(case 
	WHEN `key` = 'fab' 
	THEN pv END) as fab_pv,
MAX(case 
	WHEN `key` = 'fab' 
	THEN uv END) as fab_uv
FROM `ykt_traffic_statistics`
GROUP BY item_id;`



![](D:\code\MarkdownPhotosRepository\Res\ArticlePic\mysql列转行2.png)

