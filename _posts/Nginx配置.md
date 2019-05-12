---
title: Nginx配置
categories:
  - 技术
tags:
  - Nginx
comments: true
abbrlink: 11486
date: 2019-02-27 11:56:27
img:
---
### nginx配置


```
main                                # 全局配置

events {                            # nginx工作模式配置

}

http {                                # http设置
    ....

    server {                        # 服务器主机配置
        ....
        location {                    # 路由配置
            ....
        }

        location path {
            ....
        }

        location otherpath {
            ....
        }
    }

    server {
        ....

        location {
            ....
        }
    }

    upstream name {                    # 负载均衡配置
        ....
    }
}
```

如上述配置文件所示，主要由6个部分组成：

1.main：用于进行nginx全局信息的配置
2.events：用于nginx工作模式的配置
3.http：用于进行http协议信息的一些配置
4.server：用于进行服务器访问信息的配置
5.location：用于进行访问路由的配置
6.upstream：用于进行负载均衡的配置

### main

```
# user nobody nobody;
worker_processes 2;
# error_log logs/error.log
# error_log logs/error.log notice
# error_log logs/error.log info
# pid logs/nginx.pid
worker_rlimit_nofile 1024;
```
上述配置都是存放在main全局配置模块中的配置项

1.user用来指定nginx worker进程运行用户以及用户组，默认nobody账号运行
2.worker_processes指定nginx要开启的子进程数量，运行过程中监控每个进程消耗内存(一般几M~几十M不等)根据实际情况进行调整，通常数量是CPU内核数量的整数倍
3.error_log定义错误日志文件的位置及输出级别【debug / info / notice / warn / error / crit】
4.pid用来指定进程id的存储文件的位置
5.worker_rlimit_nofile用于指定一个进程可以打开最多文件数量的描述


