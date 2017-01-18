通过扩展 ajax 给每个请求加入 xsrf 的 header

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