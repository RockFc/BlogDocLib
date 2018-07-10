#### 将图片内嵌入Markdown文档中

将图片嵌入Markdown文档中一直是一个比较麻烦的事情。通常的做法是将图片存入本地某个路径或者网络存储空间，使用URL链接的形式插入图片： 

```
![image][url_to_image]
```

这样做一个明显的麻烦之处在于处理图片与Markdown文档的一致性上。如果我们要拷贝文档，或者图片遭到误删/云端链接失效，就会变得不便。最让我们省心的方法便是将图片直接放到文档内部。 

一个将图片嵌入文档中的方法是使用base64编码。步骤比较简单：  

1. 将图片或截图保存在本地；  
2. 使用在线工具将图片转码至base64编码；([link1](http://imgbase64.duoshitong.com/), [link2](http://tool.css-js.com/base64.html))；  
3. 在文档中插入编码： 

```
![image][data:image/png;base64, ......]
```

当然base64编码一般很长，直接将编码放入段落内部会影响正常编辑。通常的做法是将base64编码定义到一个中间变量中，将编码本体放到文档末：（示例见本站19文件存储结构网页）

```
![image][root_img]
continue edit your document here ...

[root_img]:data:image/png;base64, ......
```

使用该技巧的时候需要注意，并不是所有的Markdown编辑器都支持这种方法。而且一些Markdown编辑器只支持特定的图片格式。如有道云笔记只支持png格式的图片编码。需要在保存图片文件的时候加以注意。 