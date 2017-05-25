##通过扩展 ajax 给每个请求加入 xsrf 的 header

需要你在 html 里保存一个 _xsrf 值
```go
func (this *HomeController) Get(){        
    this.Data["xsrf_token"] = this.XsrfToken()
}
```
放在你的 head 中
```
<head>
    <meta name="_xsrf" content="{{.xsrf_token}}" />
</head>
```
扩展 ajax 方法，将 _xsrf 值加入 header，扩展后支持 jquery post/get 等内部使用了 ajax 的方法
```
var ajax = $.ajax;
$.extend({
    ajax: function(url, options) {
        if (typeof url === 'object') {
            options = url;
            url = undefined;
        }
        options = options || {};
        url = options.url;
        var xsrftoken = $('meta[name=_xsrf]').attr('content');
        var headers = options.headers || {};
        var domain = document.domain.replace(/\./ig, '\\.');
        if (!/^(http:|https:).*/.test(url) || eval('/^(http:|https:)\\/\\/(.+\\.)*' + domain + '.*/').test(url)) {
            headers = $.extend(headers, {'X-Xsrftoken':xsrftoken});
        }
        options.headers = headers;
        return ajax(url, options);
    }
});
```

对于 PUT 和 DELETE 请求（以及不使用将 form 内容作为参数的 POST 请求）来说，你也可以在 HTTP 头中以 X-XSRFToken 这个参数传递 XSRF token。

如果你需要针对每一个请求处理器定制 XSRF 行为，你可以重写 Controller 的 CheckXSRFCookie 方法。例如你需要使用一个不支持 cookie 的 API， 你可以通过将 CheckXSRFCookie() 函数设空来禁用 XSRF 保护机制。然而如果 你需要同时支持 cookie 和非 cookie 认证方式，那么只要当前请求是通过 cookie 进行认证的，你就应该对其使用 XSRF 保护机制，这一点至关重要。
##支持controller 级别的屏蔽

参考beego文档

##XXS预防
go语言提供text/template包下面的HTMLEscapeString、JSEscapeString等函数