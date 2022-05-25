---
title: "Cert Transform"
date: 2022-05-25T20:10:41+08:00
categories:
- Security
- Cert
tags:
- Cert
keywords:
- cert
- transform
- crt
- pem
clearReading: true
comments: false
showTags: true
showPagination: true
showSocial: true
showDate: true
---

This is a custom summary and does *NOT* appear in the post.
<!--more-->

{{< toc >}}

格式转换


格式说明

PKCS 全称是 Public-Key Cryptography Standards ，是由 RSA 实验室与其它安全系统开发商为促进公钥密码的发展而制订的一系列标准，PKCS 目前共发布过 15 个标准。 常用的有：
PKCS#7 Cryptographic Message Syntax Standard，常用的后缀是： .P7B .P7C .SPC
PKCS#10 Certification Request Standard
PKCS#12 Personal Information Exchange Syntax Standard，常用的后缀有： .P12 .PFX

X.509是常见通用的证书格式。所有的证书都符合为Public Key Infrastructure (PKI) 制定的 ITU-T X509 国际标准
X.509 DER 编码(ASCII)的后缀是： .DER .CER .CRT (Distinguished Encoding Rules，可辨别编码规则)
X.509 PEM 编码(Base64)的后缀是： .PEM .CER .CRT (最初是“ Privacy Enhanced Mail”的缩写)

.cer/.crt是用于存放证书，它是2进制形式存放的，不含私钥。
.pem跟crt/cer的区别是它以Ascii来表示。
pfx/p12用于存放个人证书/私钥，他通常包含保护密码，2进制方式
p10是证书请求
p7r是CA对证书请求的回复，只用于导入
p7b以树状展示证书链(certificate chain)，同时也支持单个证书，不含私钥


创建及转换

用openssl创建CA证书的RSA密钥(PEM格式)：
```shell
openssl genrsa -des3 -out ca.key 1024
```

用openssl创建CA证书(PEM格式,假如有效期为一年)：
```shell
openssl req -new -x509 -days 365 -key ca.key -out ca.crt -config openssl.cnf
```

x509转换为pfx
```shell
openssl pkcs12 -export -out server.pfx -inkey server.key -in server.crt
```

PEM格式的ca.key转换为Microsoft可以识别的pvk格式
```shell
pvk -in ca.key -out ca.pvk -nocrypt -topvk
```

PKCS#12 到 PEM 的转换
```shell
openssl pkcs12 -nocerts -nodes -in cert.p12 -out private.pem
# 验证
openssl pkcs12 -clcerts -nokeys -in cert.p12 -out cert.pem
```

PEM 到 PKCS#12 的转换
```shell
openssl pkcs12 -export -in cert.pem -out cert.p12 -inkey key.pem
```

P12 文件导出公钥私钥
```shell
# 生成key文件
openssl pkcs12 -in demo.p12 -nocerts -nodes -out demo.key
# 导出私钥
openssl rsa -in demo.key -out demo_pri.pem
# 导出公钥
openssl rsa -in demo.key -pubout -out demo_pub.pem
```

从 PFX 格式文件中提取私钥格式文件 (.key)
```shell
openssl pkcs12 -in mycert.pfx -nocerts -nodes -out mycert.key
```

转换 PEM 到 SPC
```shell
openssl crl2pkcs7 -nocrl -certfile venus.pem -outform DER -out venus.spc
```

IIS 证书
server.key和server.crt文件是Apache的证书文件，生成的server.pfx用于导入IIS
```cmd
cd c:\openssl
set OPENSSL_CONF=openssl.cnf
openssl pkcs12 -export -out server.pfx -inkey server.key -in server.crt
```

Convert PFX Certificate to PEM Format for SOAP
```shell
openssl pkcs12 -in test.pfx -out client.pem Enter Import Password: MAC verified OK Enter PEM pass phrase: Verifying - Enter PEM pass phrase:
```

PEM转DER(.crt .cer .der)
```shell
openssl x509 -in cert.pem -inform PEM -out cert.der -outform DER
```

cer(.crt .cer .der)转PEM,  然后导出公钥
```shell
# cer格式转pem格式，都是证书
openssl x509 -inform DER -in certificate.cer -out certificate.pem
# 再转公钥
openssl x509 -in certificate.pem -pubkey -noout > rsa_public_key.pem

# cer证书直接转pem公钥
openssl x509 -inform DER -in certificate.cer -pubkey -noout > rsa_public_key.pem



```

使用openssl把pem证书转换为crt和key
```shell
# pem转crt格式
openssl x509 -in fullchain.pem -out fullchain.crt  
# pem转key格式
openssl rsa -in privkey.pem -out privkey.key
```
