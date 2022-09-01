0.文件清单
layui-master:文件夹
bill.html
bill.json
bill2.html
bill2c.html
index.html
openwind.html

1.本项目主入口为bill.html,和index.html,前者为独立单据的基本操作，后者为layui的弹窗演示
2.bill.json为支持ajax请求的后台数据
3.openwind.html为支持弹窗、开窗取值返回的支持页面，可以单独运行
4.在bill.html页面下部第三个按钮，实现清空单据，并初始化单据日期和单据编号。
5.bill2c.html页面不能单独运行，是通过index页面的按钮打开的弹窗，包含的页面。
https://layui.org.cn/
https://gitee.com/sentsin/layui
http://layui-doc.pearadmin.com/doc/index.html  文档地址
https://www.layui.site/doc/modules/table.html  动态表格

关于本前端项目到服务端后台项目的跨域访问，关键注意点：
1.本前端项目在live-server服务器上运行，非vscode的插件live-server，而是使用 npm install -g live-server安装的
如果是IIS服务器上部署，另有配置，不在此说明。
2.服务器运行，使用：npm start  命令
3.本前端项目运行时的配置，支持跨域发出请求，在package.json文件中这样配置：
"scripts": {
    "start": "live-server --open=./bill.html --host=localhost --port=8080 --proxy=/api:https://192.168.0.100:5001",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
4.本项目主入口为bill.html,在页面运行时，分别发起的请求，典型如下
url: 'https://192.168.0.100:5001/api/Orderbill/GetEnd',
url:"https://192.168.0.100:5001/api/Orderbill/Nextbill?billno="+mst.billno,  //根据单号，访问下一个单
url:"https://192.168.0.100:5001/api/Orderbill/Prebill?billno="+mst.billno,    //根据单号，访问前一个单
url:"https://192.168.0.100:5001/api/Orderbill/delOrder",
url: 'https://192.168.0.100:5001/api/Orderbill/PostOrder',
等等。
5.服务端使用netcore 5开发，launchSettings.json中配置：
 "applicationUrl": "https://192.168.0.100:5001;http://192.168.0.100:5000",

6.服务端Startup.cs代码中设置：
对来自指定域的请求，给与跨域支持，比如
builder.WithOrigins("http://192.168.0.100:8080","http://localhost:8080/")
或对无差别的所有域的请求
builder.WithOrigins()

7.就这么多了，特别注意点：当从客户端--》服务端的跨域请求没有测试成功，但又觉得都配置正确，
不妨重新启动一次电脑试试。