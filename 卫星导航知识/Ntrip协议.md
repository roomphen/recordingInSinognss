#### Ntrip协议

互联网上传输RTK数据所应用的传输协议

CORS（Continuously Operating Reference Stations）就是网络基准站，通过网络收发GPS差分数据。用户访问CORS后，不用单独架设GPS基准站，即可实现GPS流动站的差分定位。

访问CORS系统，就需要网络通讯协议。NTRIP（ Networked Transport of RTCM via Internet Protocol）是CORS系统的通讯协议之一。





##### 组成：

NtripSource用来产生GPS差分数据，并把差分数据提交给NtripServer；

NtripServer 负责把GPS差分数据提交给NtripCaster；

NtripCaster差分数据中心，负责接收、发送GPS差分数据；

NtripClient登录NtripCaster后，NtripCaster把GPS差分数据发送给它；



NtripCaster建立TCP连接









