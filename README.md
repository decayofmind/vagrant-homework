# Introduction
In this homework I've tried to use the company's tool stack as much as possible.
So, it was my first time I faced with Spring and Zabbix.
Every step is automated and you can find everything in code in Ansible and everything
will be up and running after you type ```vagrant up```

# Requirements
To run this homework you'll need:

* Vagrant 1.8.5 (with VirtualBox as provider)
* Ansible 2.0+
* (optional) If you want to build Spring-boot up from source you'd rather need
Gradle and JDK installed.

# Description
## Application instance (app)

It's a Simple Spring-boot application operated by Jetty (not Tomcat), built with Gradle.
It will show you a random cat image every time you refresh the page.
Nginx reverse-proxy is used for proxying requests to Jetty.

It's accessible on http://192.168.111.11/ or http://127.0.0.1:8080/

### Metrics

Ngx_http_stub_status_module is the source of requests statistics for the app,
Zabbix uses it's data for triggering alarm.
It's also possible to grab metrics with Actuator on http://192.168.111.11/metrics or
use JMX to get them to Zabbix, but it's not implemented in this solution.

## Monitoring instance (mon)

Zabbix with MySQL (MariaDB) backend was chosen as a monitoring service. Default
installation of zabbix-web package requires Apache (httpd) and bring working config
for it, so that's why Apache is used here (not nginx).

Web interface of Zabbix can be reached at https://192.168.111.12/zabbix or at https://127.0.0.1:1443/zabbix.

Login: Admin
Password: zabbix

# Stress test
I choose a standard tool, called ab from Apache package.
To run a stress test on app:

```
vagrant ssh mon
ab -t 500 -c 20 http://192.168.111.11/
```

It will start requesting application with 20 threads within 500 sec. so alarm on
Zabbix will be triggered.
