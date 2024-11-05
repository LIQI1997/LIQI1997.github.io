# DNS

## TTL

time to live

## DNS 解析过程

1. 浏览器缓存
2. 操作系统缓存 /etc/hosts，DNS 劫持就是修改的这个文件
3. 本地网络中的 DNS 服务器
4. 本地网络中的 DNS 服务器没有命中，那它就会请求根 DNS 服务器
5. 根 DNS 服务器返回该域名对应的 Name Server
6. Name Server 就是域名提供商的服务器
7. Name Server 返回域名和 IP 记录，连同一个 TTL 值
8. 本地 DNS 服务器将解析结果返回给本地电脑

Name Server 有很多级，或者有一个 GTM 来负载均衡控制

## 域名解析记录

- A 记录：Address 记录，将域名指向一个 IP 地址

- MX 记录：Mail Exchange，邮件服务器

- CNAME 记录：Canonical Name 别名解析，指向其他的域名

- NS 记录：将域名指向一个 DNS 服务器的 IP

- TXT 记录：为域名设置说明




