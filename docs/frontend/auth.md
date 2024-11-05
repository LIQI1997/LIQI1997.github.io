# auth

第三方授权登录，使用 GitHub 测试


跳转到GitHub的页面，将本网站的授权码，重定向的链接地址，给GitHub

GitHub 登陆后，会重定向到我给他的 链接地址，并且返回 code

我拿到 code 之后，去交换用户的 token，用户拿到 token 后就可以访问 GitHub 的 api 了。

为什么 GitHub 在登录成功后，不直接把用户的 token 给返回了，因为安全？
