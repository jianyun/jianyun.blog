title: nginx jetty https
date: 2018-04-27 10:26:19
tags: [nginx,jetty,https]
category: [https]
---
## nginx配置https
    server {
        listen 443 ssl;
        server_name www.nbbdw.com;
        ssl on;
        ssl_certificate /home/sslkey/nbbdw.crt;
        ssl_certificate_key /home/sslkey/nbbdw.key;

        proxy_set_header Host $http_host;
        proxy_set_header X-real-ip $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Http-scheme https;
        proxy_redirect http:// $scheme://;

        location / {
                proxy_pass http://portalsite;
        }
      }

## 问题
    nginx到jetty是http，request.getScheme()一直取到http；request.getRequestURL()也是http。

## 解决方法

    修改jetty（9）的start.ini配置

    --module=http-forwarded

    完美解决。

## 实际用到

     <?xml version="1.0"?>
      <!DOCTYPE Configure PUBLIC "-//Jetty//Configure//EN" "http://www.eclipse.org/jetty/configure_9_3.dtd">
      <Configure id="httpConfig" class="org.eclipse.jetty.server.HttpConfiguration">
        <Call name="addCustomizer">
          <Arg>
            <New class="org.eclipse.jetty.server.ForwardedRequestCustomizer">
              <Set name="forwardedOnly"><Property name="jetty.httpConfig.forwardedOnly" default="false"/></Set>
              <Set name="proxyAsAuthority"><Property name="jetty.httpConfig.forwardedProxyAsAuthority" default="false"/></Set>
              <Set name="forwardedHeader"><Property name="jetty.httpConfig.forwardedHeader" default="Forwarded"/></Set>
              <Set name="forwardedHostHeader"><Property name="jetty.httpConfig.forwardedHostHeader" default="X-Forwarded-Host"/></Set>
              <Set name="forwardedServerHeader"><Property name="jetty.httpConfig.forwardedServerHeader" default="X-Forwarded-Server"/></Set>
              <Set name="forwardedProtoHeader"><Property name="jetty.httpConfig.forwardedProtoHeader" default="X-Forwarded-Proto"/></Set>
              <Set name="forwardedForHeader"><Property name="jetty.httpConfig.forwardedForHeader" default="X-Forwarded-For"/></Set>
              <Set name="forwardedHttpsHeader"><Property name="jetty.httpConfig.forwardedHttpsHeader" default="X-Proxied-Https"/></Set>
              <Set name="forwardedSslSessionIdHeader"><Property name="jetty.httpConfig.forwardedSslSessionIdHeader" default="Proxy-ssl-id" /></Set>
              <Set name="forwardedCipherSuiteHeader"><Property name="jetty.httpConfig.forwardedCipherSuiteHeader" default="Proxy-auth-cert"/></Set>
            </New>
          </Arg>
        </Call>
      </Configure>