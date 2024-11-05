# SSO

sso
single sign on
在多个业务系统中，只需要登录一次，就可以访问其他相互信任的业务系统。

传统的登录
用户登录 app.test.com，服务端保存 session id，保存在 cookie 中，用户拿着 cookie 访问即可。
不同的网站，都是同一种逻辑，只不过 session id 和 cookie 只在它那个域名下有用，因为 session 肯定是独立的，cookie 也不能跨域。

同域名单点登录，可以实现，只需要做到把 cookie 保存在顶域中，把不同服务端的 session 共享即可，比如使用 spring-session。

不同域名单点登录：CAS 流程，标准流程
app1.test.com, app2.test.com, sso.test.com

用户访问 app1.test.com，重定向到 sso.test.com，也就是 sso 系统，也叫做 CAS Server

sso 系统如果没登录，显示登录表单页面，让用户登录，用户登陆后，sso系统写入 session，生成 ST（service ticket），携带这个参数跳转回 app1.test.com

app1.test.com 拿着这个 ST 去请求 sso 系统验证是否有效，如果有效，在本地记录下 sso 的 session_id 到自己域下的 cookie，即可访问数据。

同样道理，app2 也是重定向到 sso 系统，如果已在 sso 系统登录，那就直接返回 ST，app2 去给 sso 系统发请求验证 ST，验证成功就可以愉快地上网了。

为什么 sso 系统登陆成功后，还需要业务系统拿着 ST 去验证呢？直接把用户的 token 给业务系统，让业务系统直接请求业务，不就可以了么？这样就意味着用户可以猜别人的token ？



自己实现一套单点登陆方案，允许第三方传递 url 参数给我

sso
每个页面进入之前都会加载 sso js，等待 sso 结束才会开始渲染
sso 最开始判断当前页面是否是 centerlogin，如果是，直接结束。
如果 url 中有 token，保存在 local storage 中，结束。


## jwt

用户登录成功后，给用户签发 JWT 令牌，设置 refresh_token，并将 refresh_token 保存在数据库中，并设置过期时间。 

当用户访问需要验证身份的接口时，在请求头中携带 JWT 令牌。

服务器收到请求后，首先验证 JWT 令牌的有效性，如果令牌有效，则继续处理请求；如果令牌无效，若 refresh_token 有效，则返回 refresh_token，

如果 refresh_token 失效，则返回错误信息，让用户重新登录。

正常情况下，可以把用户的令牌设置时间很短，比如1小时，如果用户个人信息，权限等没有发生变化，则可以使用 refresh_token 来刷新令牌，延长用户的登录时间。

如果发生了变化，则需要重新登录。

refresh_token 使用一次后，就不可以再使用了。