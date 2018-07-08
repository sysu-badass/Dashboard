## SYSU SE-308 软件综合实训

#### 目录
[软件设计文档](https://github.com/sysu-badass/Dashboard/blob/master/Documents/%E7%BB%BC%E5%90%88%E5%AE%9E%E8%AE%AD%E6%96%87%E6%A1%A3/%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3.md)

[后台代码](https://github.com/sysu-badass/BackEndServer)

[商户PC端代码](https://github.com/sysu-badass/merchant-end-web-page)

## 系统分析与设计

#### 目录
1. [About  (项目规划)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/about.md)
2. [Team profile (团队组建)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/Team-profile.md)
3. [Investigation (项目前期调研)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/Investigation-report.md)
4. [Vision (项目愿景)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/Vision.pdf)
5. [Product Backlog (产品特性)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/backlog.md)
6. [Requirement specification (需求规格说明)](https://github.com/sysu-badass/Dashboard/tree/master/Documents/Requirement-specification)
	- 6.1 [Usecase Diagram (用例图)](https://github.com/sysu-badass/Dashboard/tree/master/Documents/Requirement-specification/Usecase-Diagram)
	- 6.2 [Use Cases (用例+活动图)](https://github.com/sysu-badass/Dashboard/tree/master/Documents/Requirement-specification/Use-cases)
	- 6.3 [Domian Model (领域模型)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/Requirement-specification/Domain-Model.png)
	- 6.4 [State Model (状态模型)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/Requirement-specification/State-Model/State-Model.md)
	- 6.5 [System Sequence Diagram (功能模型)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/Requirement-specification/System-Sequence-Diagram.md)
	- 6.6 补充需求
7. Design (设计)
	- 7.1 [UI design](https://github.com/sysu-badass/Dashboard/blob/master/Documents/UI-design.md)
	- 7.2 [Database design](https://github.com/sysu-badass/Dashboard/tree/master/Documents/database_design)
	- 7.2.1 [用户及权限系统数据库设计](https://github.com/sysu-badass/Dashboard/blob/master/Documents/database_design/%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%95%B0%E6%8D%AE%E5%BA%93%E8%AE%BE%E8%AE%A1.png)
		- 7.2.2 [点餐系统数据库设计](https://github.com/sysu-badass/Dashboard/blob/master/Documents/database_design/%E7%82%B9%E9%A4%90%E7%B3%BB%E7%BB%9F%E6%95%B0%E6%8D%AE%E5%BA%93%E8%AE%BE%E8%AE%A1.png)
		- 7.2.x 第三方数据评审结果 (under construction)
	- 7.3 [API 设计](https://github.com/sysu-badass/Dashboard/blob/master/Documents/API%E8%AE%BE%E8%AE%A1.md)
	- 7.4 [Software Architecture Document](https://github.com/sysu-badass/Dashboard/blob/master/Documents/Requirement-specification/Software-Architecture-Document.md)
	- 7.5 Usecase design

8. [生产规范与指南](https://github.com/sysu-badass/Dashboard/tree/master/Documents/%E7%94%9F%E4%BA%A7%E8%A7%84%E8%8C%83%E4%B8%8E%E6%8C%87%E5%8D%97)
	- 8.1 [python代码规范](https://github.com/sysu-badass/Dashboard/blob/master/Documents/%E7%94%9F%E4%BA%A7%E8%A7%84%E8%8C%83%E4%B8%8E%E6%8C%87%E5%8D%97/python%E4%BB%A3%E7%A0%81%E8%A7%84%E8%8C%83.md)
	- 8.2 [REST API 设计规范](https://github.com/sysu-badass/Dashboard/blob/master/Documents/%E7%94%9F%E4%BA%A7%E8%A7%84%E8%8C%83%E4%B8%8E%E6%8C%87%E5%8D%97/REST_API%E8%AE%BE%E8%AE%A1%E8%A7%84%E8%8C%83.md)
	- 8.3 [框架目录设计与逻辑架构与 ECB 的关系](https://github.com/sysu-badass/Dashboard/blob/master/Documents/ECB.md)
	- 8.4 [物理架构云上部署 dock-compose.yml 文件编写与使用](https://github.com/sysu-badass/Dashboard/blob/master/Documents/deployment.md)

9. demo运行效果
	- 商户PC端
		- [登录注册](http://pb1ftb8nx.bkt.clouddn.com/%E7%99%BB%E5%BD%951.gif)
		- [登录](http://pb1ftb8nx.bkt.clouddn.com/%E7%99%BB%E5%BD%95%E8%BF%9B.gif)
		- [不同面板](http://pb1ftb8nx.bkt.clouddn.com/%E5%90%84%E7%A7%8D%E8%B7%B3%E8%BD%AC.gif)
		- [修改商家信息](http://pb1ftb8nx.bkt.clouddn.com/%E4%BF%AE%E6%94%B9info.gif)
		- [订单管理](http://pb1ftb8nx.bkt.clouddn.com/%E8%AE%A2%E5%8D%95%E7%AE%A1%E7%90%86.gif)
		- [菜单界面](http://pb1ftb8nx.bkt.clouddn.com/%E8%8F%9C%E5%93%81%E7%AE%A1%E7%90%861.gif)
		- [添加菜品](http://pb1ftb8nx.bkt.clouddn.com/%E6%B7%BB%E5%8A%A0%E8%8F%9C%E5%93%81.gif)
	- 顾客小程序端
		- [查看餐品详细信息](https://github.com/sysu-badass/wechat-end-web-page/raw/master/performance/perform3.gif)
		- [添加餐品到购物车](https://github.com/sysu-badass/wechat-end-web-page/raw/master/performance/perform1.gif)
		- [查看个人信息以及历史订单](https://github.com/sysu-badass/wechat-end-web-page/raw/master/performance/perform2.gif)


X1 [meet_recording](https://github.com/sysu-badass/Dashboard/tree/master/Documents/meeting-record)
  - [inception meeting (2018/03/26)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/meeting-record/inception-meeting.md)
  - [iteration1 meeting(2018/04/14)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/meeting-record/iteration1-meeting.md)
  - [iteration3 meeting(2018/05/10)](https://github.com/sysu-badass/Dashboard/blob/master/Documents/meeting-record/iteration2-meeting.md)

X2 Tech/Work Report
  - [1215331052 - 使用墨刀进行原型设计](https://chengr25.github.io/2018/04/15/lesson5/)
  - [15331177 - flask入门](https://ishoping.github.io/hw5/)
  - [15331085 - QRCode Learning](https://8652.github.io/QR-Code/)
  - [15331047 - python3 decorator](https://saltyfish123.github.io/15331047_homework_3/)
  - [15331064 - 使用git上传文件至项目仓库](https://blog.csdn.net/qq_33361432/article/details/79919040)

X3 Final Report
  - [15331047-FinalReport](https://saltyfish123.github.io/15331047_final_report/)
