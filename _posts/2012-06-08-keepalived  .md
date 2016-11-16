---
layout: post
title:  "  keepalived 检测  "
categories: keepalived
tags:  keepalived
excerpt:    keepalived 检测  
---

通常说的双机热备是指两台机器都在运行，但并不是两台机器都同时在提供服务。
当提供服务的一台出现故障的时候，另外一台会马上自动接管并且提供服务，而且切换的时间非常短。

主服务器：192.168.1.118

从服务器：192.168.1.192

虚拟ip:192.168.1.223


[html] 

    yum install keepalived  


修改主服务器配置文件

[python] 

    vim  /etc/keepalived/keepalived.conf  


[python] 

    ! Configuration File for keepalived  
      
       
      
    global_defs {  
      
       notification_email {  
      
         #acassen@firewall.loc没有服务器配置邮箱可将其注释掉  
      
         #failover@firewall.loc  
      
         #sysadmin@firewall.loc  
      
       }  
      
       #notification_email_from Alexandre.Cassen@firewall.loc  
      
       #smtp_server 192.168.200.1  
      
       #smtp_connect_timeout 30  
      
       router_id LVS_DEVEL  
      
    }  


 

[python] 

    vrrp_instance VI_1 {  
      
        state MASTER  
      
        interface eno16777736  
      
        virtual_router_id 51#和slave一样  
      
        priority 100#主机高于slave  
      
        advert_int 1#检测服务器状态间隔时间  
      
        authentication {  
      
            auth_type PASS  
      
            auth_pass 1111  
      
        }  
      
        virtual_ipaddress {  
      
            192.168.1.223#虚拟IP地址，可以为多个  
      
        }  
      
    }  



开启服务

[php] 

    systemctl start keepalived  



修改slave配置

! Configuration File for keepalived

global_defs {
   notification_email {
     acassen@firewall.loc
     failover@firewall.loc
     sysadmin@firewall.loc
   }
   notification_email_from Alexandre.Cassen@firewall.loc
   smtp_server 192.168.200.1
   smtp_connect_timeout 30
   router_id LVS_DEVEL
}

vrrp_instance VI_1 {
    state BACKUP
    interface eno16777736
    virtual_router_id 51
    priority 99
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }

 virtual_ipaddress {
        192.168.1.223
    }
}

在两台机器上使用 "ip a"查看虚拟IP信息

就可以比较出来了.祝各位成功.


还有我们使用yum安装的. 还有一点就是

rpm -ql keepalived

vi /usr/share/doc/keepalived-1.2.13/samples/keepalived.conf.vrrp.localcheck


这个里面有例子。


 

