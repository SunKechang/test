# test_vue

## 启动步骤
1. npm install
2. npm run serve

## 详细说明
首页有两个按钮，登录、写入数据。分别设计两个云函数：adminLogin和addOne。

因为这里主要模拟管理员后台登录和写入数据两个操作。所以把

登录按钮会携带用户名、密码请求云函数adminLogin，获取ticket。得到ticket后调用微信的服务来获取uid。得到uid后就说明登录成功。具体操作参考：https://cloud.tencent.com/document/product/876/41731

另一个按钮用于向skc-test数据库写入数据，模拟管理员添加一条数据。在云函数界面点击“权限控制”，设置addOne函数为只有登录用户才能调用。
![image](public/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20230227161413.png)

然后客户端请求的时候header需要加上`x-cloudbase-credentials`字段，具体操作看参考。（参考：https://docs.cloudbase.net/service/authentication）

剩下的数据库操作就在云函数里完成了。

其中管理员登录用的**自定义登录**，有别于**微信登录**。