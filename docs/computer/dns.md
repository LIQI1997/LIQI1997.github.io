# DNS

## DNS 解析过程

1. 浏览器缓存
2. 操作系统缓存，如 Linux 下的 /etc/hosts 配置文件，DNS 劫持就是修改这个文件来实现的
3. 局域网的 DNS 服务器没有命中，那它就会请求根 DNS 服务器
4. 根 DNS 服务器返回该域名对应的 Name Server
5. Name Server 就是域名提供商的服务器，Name Server 返回域名和 IP 记录，连同一个 TTL 值
6. 局域网 DNS 服务器将 Name Server 返回的结果再返回给用户电脑的操作系统缓存
7. 操作系统缓存再给浏览器缓存

Name Server 有很多级，或者有一个 GTM 来控制负载均衡。

## TTL

time to live

## 域名解析记录

- A 记录：Address 记录，将域名指向一个 IP 地址
- MX 记录：Mail Exchange 记录，邮件服务器
- CNAME 记录：Canonical Name 别名解析，指向其他的域名
- NS 记录：将域名指向一个 DNS 服务器的 IP
- TXT 记录：为域名设置说明




