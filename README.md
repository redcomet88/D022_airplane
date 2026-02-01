# D022 vue+django+neo4j 航班飞机知识图谱与问答 | LTP+neo4j |飞机知识问答

> 完整项目收费，可联系QQ: 81040295 微信: mmdsj186011 注明从git来的，谢谢！
也可以关注我的B站： 麦麦大数据 https://space.bilibili.com/1583208775
> 

关注up主B站：  **麦麦大数据**
关注B站，有好处！
编号:  D022
## 视频

[video(video-7LEzwg8g-1760323832631)(type-bilibili)(url-https://player.bilibili.com/player.html?aid=113422184219636)(image-https://i-blog.csdnimg.cn/img_convert/5d6195d48e197855811cf77e60371eb3.jpeg)(title-vue+django+neo4j航班智能问答知识图谱可视化系统)]

## 1 系统简介
系统简介：本系统是一个基于Vue+Flask构建的高考预测可视化系统，其核心功能围绕高考数据的展示、预测和用户管理展开。主要包括：首页，用于展示系统概览和轮播图；数据卡片，提供高考数据的概览，并支持查看位置（例如通过百度地图）及用户点赞“喜欢”的功能；可视化模块，通过丰富的图表展示专业的排名信息和关注度热力图，为用户提供直观的数据分析；分数线预测模块，利用机器学习模型进行智能预测，为考生提供参考；以及用户管理模块，包含登录与注册功能，和个人设置（允许用户修改个人信息、头像及密码），确保系统的安全性和个性化体验。
## 2 功能设计
该系统采用典型的B/S（浏览器/服务器）架构模式。用户通过浏览器访问Vue前端界面，该前端由HTML、CSS、JavaScript以及Vue.js生态系统中的Vuex（用于状态管理）、Vue Router（用于路由导航）和Echarts（用于数据可视化）等组件构建。前端通过API请求与Django后端进行数据交互，Flask后端则负责业务逻辑处理，并利用SQLAlchemy（或类似ORM工具）与MySQL数据库进行持久化数据存储。此外，系统还包含一个独立的爬虫模块，负责从外部来源抓取数据并将其导入MySQL数据库，为整个系统提供数据支撑。
### 2.1系统架构图
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/950e50bd16fa4a0098d7d181c5a80998.png)
### 2.2 功能模块图
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/34e69b03f4e04f73b0285e84e60c233b.png)
## 3 功能展示
### 3.1 登录 & 注册
登录界面背景是一个视频，展示和本文系统主题相匹配的内容，登录和注册界面在一个界面下，通过按钮来切换，注册界面输入用户名和密码，会检查这个用户是否存在，登录界面则要检查用户名是否存在以及用户名密码是否正确：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bf4e156a73e64f7bb2fdae0dbbedca47.jpeg)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7ac0e5700ebd45ad9c31a91483f91a60.jpeg)
### 3.2 主页
如果通过校验，则可以进入主页，在主页是一个左侧菜单，右侧操作面板的布局，右上角是登录用户的头像和退出按钮：
### 3.3 知识图谱可视化
通过python代码实现对知识图谱的neo4j导入：
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e570245e4249455eba3d5f6dd5e8fab5.png)
在系统内实现对neo4j知识图谱的可视化展示，采用echarts关系图作为可视化的方案：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4b57c80f0bd14883b39dd7085821880c.jpeg)
支持输入关键词的模糊检索：
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a51c99dd8c734a46a576874426020f57.jpeg)
### 3.4 智能问答
实现对航班知识的问答，通过聊天式界面实现
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fbdb79b5dd5641c8b02f3c9732af42a0.jpeg)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1e5d13dcd156462691fccd59cb12c79b.jpeg)
### 3.5 数据处理
航班数据：
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/48541389cdfb405b9eac1cb132b94057.jpeg)

航班数据导入过程：
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bda5b6ec3185483e944cb4470d684634.jpeg)
## 4程序代码
### 4.1 代码说明
代码介绍：该系统基于哈工大LTP模型，通过自然语言处理技术从用户的问句中提取关键信息（如起始地、目的地、出行日期等），然后查询知识图谱以获取相关的航班信息。系统的核心流程包括以下步骤：

问句解析：使用LTP模型对用户输入的问句进行分词、词性标注和句法分析，提取出关键词。
知识图谱查询：根据提取的关键词从知识图谱中检索相关的航班信息。
结果生成：根据查询结果生成自然语言的回答。
### 4.2 流程图
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2802381eff3949cdbb1a610c19f4c018.png)

### 4.3 代码实例
```python
import hanlp
# 初始化LTP模型
nlp = hanlp.load("LTP")
def process_question(question):
    # 分词和词性标注
    tokens = nlp.tokenize(question)
    # 提取关键词（如地点、日期等）
    keywords = extract_keywords(tokens)
    # 查询知识图谱
    results = query_knowledge_graph(keywords)
    # 生成回答
    return generate_response(results)
```
