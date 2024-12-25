# HTML 打开新网页

- window.location.replace

  在当前网页打开新网页

- window.open

  在新标签页打开网页
  
- window.location.href

  直接修改当前页面的url在url中传递参数

  - 页面1

    ```js
    function Post(){
        //多参值 Read.html?username=baobao&sex=male;
        url="Read.html?username="+escape(document.all.username.value);
        url+="&sex="+escape(document.all.sex.value);
        location.href=url;
    }
    ```

    采用GET类型的http请求

  - 页面2

    ```js
    var url = location.search;
    // url地址
    var Request = new Object();
    if(url.indexOf("?")!=-1){
        var str = url.substr(1)//去除?号
        strs = str.sqlit("&");
        for(var i = 0;i<strs.length;i++){
            Request[strs[i].split("=")[0]]=unescape(strs[i].split("=")[1]);
        }
    }
    alert(Request["username"]);
    alert(request["sex"]);
    ```

    从url中获取信息

