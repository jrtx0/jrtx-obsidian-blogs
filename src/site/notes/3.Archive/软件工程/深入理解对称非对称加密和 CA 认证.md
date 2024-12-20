---
{"dg-publish":true,"dg-path":"软件工程/深入理解对称非对称加密和 CA 认证.md","permalink":"/软件工程/深入理解对称非对称加密和 CA 认证/","created":"2023-11-24T16:33:36.000+08:00","updated":"2024-08-31T22:07:39.000+08:00"}
---

#Technomous

# 对称加密

对称加密的过程如下图所示。通信的双方约定好使用统一的加密解密算法，以及一个 salt 盐作为唯一标识，发送数据前先使用加密算法和 salt 经过加密函数处理得到密文，接受方收到密文后使用解密算法和 salt 对密文解密得到明文再处理。

![Pasted image 20231124163535.png|350](/img/user/0.Asset/resource/Pasted%20image%2020231124163535.png)

## 漏洞分析

黑客可能会枚举出对称加密算法，而且 salt 是唯一的，不会为不同的服务创建不同的 salt。一旦出现信息泄漏，很可能出现如下情况：客户端和服务端之间的数据被黑客窃取并篡改，再将篡改后的数据发给服务端。因为黑客知道完整的加解密方法和 salt，所以他能瞒天过海。

![Pasted image 20231124163650.png|350](/img/user/0.Asset/resource/Pasted%20image%2020231124163650.png)

# 非对称加密

非对称加密通过公钥和私钥实现。它有以下特点：

1. 公钥加密的数据只有私钥才能解密，公钥自己是解密不了的。
2. 私钥加密的数据只有公钥才能解密，私钥自己是解密不了的。
3. 服务端同时持有公钥和私钥，不会发送给其他人。
4. 服务端要跟谁通信就把自己的公钥给它。

使用非对称加密的交互的过程如下：客户端先拿到服务端的公钥，然后使用这个公钥加密数据，再把加密后的数据发送给服务端。根据上面说到的 1、2 两点，这时只有服务端才能正确的解密出数据。

![Pasted image 20231124163809.png|350](/img/user/0.Asset/resource/Pasted%20image%2020231124163809.png)

## 漏洞分析

如下红色部分所示，服务端发送给客户端的数据如果使用公钥加密，那么客户端肯定解密不了，看起来它只能使用私钥加密，这时客户端可以使用之前获取到的公钥解密。但是问题是所有人都能获取到服务端的公钥，包括黑客。所以黑客也能解密出服务端发送过来的数据。

![Pasted image 20231124163910.png|350](/img/user/0.Asset/resource/Pasted%20image%2020231124163910.png)

# 对称加密和非对称加密结合

两者结合的方式如下，客户端先获取到服务端的公钥，然后自己生成一个唯一的随机密钥 A，使用公钥加密随机密钥 A，这时只有服务端的私钥才能解密出随机密钥 A，所以即便被加密的随机密钥 A 被黑客截获它也解密不出啥。服务端拿到随机密钥 A 之后，服务端和客户端双方约定从此之后双方的交互使用随机密钥 A 做对称加密的 salt，全球只有他俩知道，所以很安全。

![Pasted image 20231124163957.png|350](/img/user/0.Asset/resource/Pasted%20image%2020231124163957.png)

## 漏洞分析

如下图所示，假设黑客很厉害，他有自己的公钥和私钥，而且从一开始就截取了客户端的请求，然后自己冒充服务端。在客户端和服务端的交互过程中全程充当一个代理的存在，这样黑客依然能获取到双方交互的所有数据。简而言之这个问题就出在：客户端太信任他拿到的公钥了。

![Pasted image 20231124164055.png|550](/img/user/0.Asset/resource/Pasted%20image%2020231124164055.png)

# HTTPS 的做法-引入 CA 机构

为了解决上面说到的问题：客户端太信任他拿到的公钥。所以我们引入了第三方的 CA 机构。CA 机构出现之后，所有人就约定：我们只相信被 CA 机构信任的公钥（也就是下面说到的证书）

![Pasted image 20231124164141.png|550](/img/user/0.Asset/resource/Pasted%20image%2020231124164141.png)

从上图可以看出，CA 机构有自己的公钥和私钥，大家绝对信任 CA 认证机构，让他做安全方面的背书。这时黑客再想插进入比如偷偷替换服务端的公钥，那不好意思，客户端只相信权威机构的公钥能解析的证书。即便黑客自己也有 CA 机构颁发给他自己的证书，客户端也不会认，因为证书是和域名绑定的，而域名是唯一的。