在web2.0时代，熟练的使用ajax是每个前端工程师必备的技能，然而由于收到浏览器的限制，ajax不允许跨域通信。JSONP就是目前主流的实现跨域通信的解决方法

虽然在jq中，我们可以通过$.ajax的dataType设置为jsonp来调用jsonp，jsonp主要是通过script可以链接远程url来实现跨域请求的。如：

```
<script src="http://jsonp.js?callback=xxx"></script>
```

callback定义了一个函数名，而远程服务通过调用指定的函数并传入参数来实现传递参数。

```
var JSONP = {
  //获取当前时间戳
  now: function() {
    return (new Date()).toString.substr(2);
  },

  //删除节点元素
  removeElem: function(elem) {
    var parent = elem.parentNode;
    if(parent && parent.nodeType !== 11) {
      parent.removeChild(elem);
    }
  },

  //url组装
  parseData: function(data) {
    var ret = "";
    if(typeof data === "string") {
      ret = fata;
    }else if(typeof data === 'object') {
      for(var key in data) {
        for(var key in data) {
          ret += '&' + key + "=" + ecodeURIcomponent(data[key]);
        }
      }
      //加个时间戳，防止缓存
      ret += "&_time=" + this.now();
      ret = ret.substr(1);
      return ret;
    }
  },

    getJSON: function(url, data, func){
      //函数名称
      var name;

      //拼装url
      url = url + (url.indexOf("?") === -1 ? "?" : "&") + this.parseData(data);

      //检测callback的函数名是否已经定义
      var match = /callback=(\w+)/.exec(url);
      if(match && match[1]) {
        name = match[1];
      } else {
        //如果未定义函数名的话随机生成一个函数名
        //随机生成的函数名通过时间戳拼16位随机数的方式，重名的概率基本为0
        //如:jsonp_1355750852040_8260732076596469
        name = "jsonp_" + this.now() + "_" + this.rand();
        // 把callback中的？替换成函数名
        url = url.replace("callback=?", "callback=" + name);
        // 处理？被encode的情况
        url = url.replace("callback=%3F", "callback=" + name);
      }

      //创建一个script元素
      var script = document.createElement("script");
      script.type = "text/javascript";
      //设置远程的url
      script.src = url;
      //设置id，为了后面可以删除这个元素
      script.id = "id_" + name;

      //把传进来函数重新组装，并把它设置为全局函数，远程就是调用这个函数
      window[name] = function(json) {
        //执行这个函数后，要销毁这个函数
        window[name] = undefined;
        //获取这个script的元素
        var elem = document.getElementById("id_" + name);
        //删除head中插入的script，这三步都是为了不影响污染整个DOM
        JSONP.removeElem(elem);
        //执行传入的函数
        func(json);
      };

      //在head里面插入script元素
      var head = document.getElementByTagName("head");
      if(head && head[0]) {
        head[0].appendChild(script);
      }
  }
}
```

调用的方法

```
var data = {...}
JSONP.getJSON("http://api.qunar.com/cdnWebService.jsp", data, function(json) {console.log(json)});
//或者这样
JSONP.getJSON("http://api.qunar.com/cdnWebservices.jcp?from=北京&count=27&output=json&callback=?", null, function(json) {console.log(json)});
```
