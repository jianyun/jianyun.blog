title: android浏览器https空白
date: 2018-05-04 18：39
tags: [android,https]
category: [https]
---

## 问题
    nginx配置https,pc和苹果浏览器正常显示，android浏览器提示证书链不完整，微信浏览器空白

## 解决方法

   查看百度证书，链路对照 openssl s_client -connect www.baidu.com:443   openssl s_client -connect www.xxx.com:443
   明显看到证书缺失，error


   查看证书购买邮件，将网站证书、服务证书、根证书按以下合并，刷新nginx完美解决
   -----BEGIN CERTIFICATE-----
   (Your Primary SSL certificate: your_domain_name.crt)
   -----END CERTIFICATE-----
   -----BEGIN CERTIFICATE-----
   (Your Intermediate certificate: DigiCertCA.crt)
   -----END CERTIFICATE-----
   -----BEGIN CERTIFICATE-----
   (Your Root certificate: TrustedRoot.crt)
   -----END CERTIFICATE-----


