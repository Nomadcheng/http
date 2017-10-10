# cookie及其操作

- cookie是wenb浏览器存储的少量数据，最早设计为服务器端使用，作为HTTP协议的扩展实现，cookie数据会自动在浏览器和服务器之间传输
- 通过读写cookie检测是否支持
- cookie属性有name,value,max-age,path,domain,secure
- cookie默认有效期为浏览器会话，一旦用户关闭浏览器，数据就丢失，通过设置max-age=secondess属性告诉浏览器cookie的有效期
- cookie作用域通过文档源和文档路径来确定，通过path和domain进行配置，web页面同目录或子目录文档都可访问
- 通过cookie保存数据的方法为：为document.cookie设置一个符合目标的字符串如下
- 读取document.cookie获得'; '分隔的字符串，key=value,解析得到结果

```
document.cookie = 'name=qiu; max-age=9999; path=/; domain=domain; secure';

document.cookie = 'name=aaa; path=/; domain=domain; secure';
// 要改变cookie的值，需要使用相同的名字、路径和域，新的值
// 来设置cookie，同样的方法可以用来改变有效期

// 设置max-age为0可以删除指定cookie

//读取cookie，访问document.cookie返回键值对组成的字符串，
//不同键值对之间用'; '分隔。通过解析获得需要的值
```

## 封装cookie操作的函数

[cookieUtil](https://github.com/Nomadcheng/jsCoding/blob/master/javascriptDOM/cookieUtil.js)
