Ajax是对Asynchronous Javascript + XML的简写，这一技术能够向服务器请求额外的数据而无须卸载页面

Ajax的核心是XMLHttpRequest对象（XHR），XHR为向服务器发送请求和解析服务器响应提供了流畅的接口，能够以异步方式从服务器获取新的数据，而不必刷新页面

```
//使用IE7之前的版本
function createXHR() {
  if(typeof XMLHttpRequest != "undefined") {
    return new XMLHttpRequest();
  } else if (typeof ActiveXObject != "undefined"){
    if(typeof arguments.callee.ActiveXObject != "string") {
      var version = [ "MSXML2.XMLHttp.6.0", "MSXML2.XMLHttp.3.0", "MSXML2.XMLHttp"],
          i,
          len;
      for (i=0, len=versions.length; i < len; i++){
        try {
          new ActiveXObject(versions[i]);
          arguments.callee.activeXString = versions[i];
          break;
        } catch (ex) {
          ...
        }
      }
      return new ActiveXObject(arguments.callee.activeXString);
  } else {
    thrw new Error("No XHR Object avaiable");
  }
}
```

调用

```
var xhr = createXHR();
xhr.open("get", "example.php", false);
//添加http头部信息
xhr.setRequestHeader("Content-type", "application/json");
xhr.onreadystatechange = function() {
<!-- 0 － （未初始化）还没有调用send()方法 
1 － （载入）已调用send()方法，正在发送请求
2 － （载入完成）send()方法执行完成，已经接收到全部响应内容
3 － （交互）正在解析响应内容
4 － （完成）响应内容解析完成，可以在客户端调用了 -->
  //要调用onreadystatechange事件处理程序才能确保浏览器的兼容性
  if(xhr.readyState == 4){
    //304表示请求资源并没有被修改，可以直接使用浏览器中缓存的版本
    if ((xhr.status >= 200 && xhr.status < 300 )|| xhr.status == 304){
      alert(xhr.responseText);
    } esle {
      alert("Request was unsuccessful: " + xhr.status);
    }
  }
};
//这里的send方法接收一个参数作为请求主体发送的数据，如果不需要通过请求主体发送数据，则必须传入null
xhr.send(null);
```

对xhr而言，位于传入open()方法的URL末尾的查询字符串必须经过正确的编码才行，使用get请求经常发生的一个错误就是查询字符串的格式有问题，查询字符串中每个参数发的名称和值必须经过encodeURIComponent()进行编码，然后才能放到URL末尾，而且所有名-值对都必须由和号（&）分隔

```
function addURIParam(url, name, value) {
  url += (url.indexOf("?") === -1 ? "?" : "&");
  url += encodeURIComponent(name) + "=" + encodeURIComponent(value);
  return url;
}
```
