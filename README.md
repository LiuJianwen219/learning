以下说明基于 ubuntu-server 14.04 环境。

## 一、 创建用户

添加用户 exotic 并赋予其 `sudo` 权限。

## 二、 Python环境

项目基于 Python3.5 。

```bash
# 防止 ImportError: cannot import name 'HTTPSHandler'
$ sudo apt-get install libssl-dev openssl

$ wget https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tgz
$ tar xzvf Python-3.5.2.tgz
$ cd Python-3.5.2

# 防止 zlib module is not available
$ ./configure
$ cd Modules    # Python-3.5.2/Modules
$ vim Setup     # and uncomment the line: zlib zlibmodule.c -I$(prefix)/include -L$(exec_prefix)/lib -lz
$ cd zlib       # Python-3.5.2/Modules/zlib
$ ./configure
$ sudo make install

$ cd ../..      # Python-3.5.2
$ make
$ sudo make install
```

### pip

若系统未提供 pip 工具，可通过以下指令安装。

```
$ wget https://bootstrap.pypa.io/get-pip.py
$ sudo python3 get-pip.py
```

### virtualenv

```
$ sudo pip install virtualenv
```

## 三、 node.js环境

项目基于 node.js 6.2.0 ，建议通过 [nvm](https://github.com/creationix/nvm) 安装。

``` bash
$ wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.31.7/install.sh | bash
$ source ~/.bashrc
$ nvm install 6.2.0
```

## 四、 syslog

新建 `/etc/rsyslog.d/exotic-server.conf` ，添加以下内容：

```
if $syslogfacility-text == 'local2' and $programname == 'exotic_server' then /var/log/exotic-server/syslog
```

创建日志文件夹。

```
$ mkdir -p /var/log/exotic-server
$ chown syslog:admin /var/log/exotic-server
```

然后重启 `rsyslog`。

```
$ sudo serivice rsyslog restart
```

如果无法成功创建日志，可能为权限问题。`/etc/rsyslog.conf` 中相关配置为：

```
$PrivDropToUser syslog
$PrivDropToGroup syslog
```

## 五、 Nginx

下载并解压 `zlib-1.2.8`、`pcre-8.38`、`nginx-upload-progress-module`、`nginx-rtmp-module`。

下载 Nginx 源码包。

在解压后的根目录下编译安装。注意 `configure` 时根据实际情况修改最后四行模块地址。

```
$ ./configure  \
    --prefix=/etc/nginx \
    --sbin-path=/usr/sbin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --pid-path=/var/run/nginx.pid \
    --lock-path=/var/run/nginx.lock \
    --http-client-body-temp-path=/var/cache/nginx/client_temp \
    --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
    --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
    --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
    --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
    --user=nginx \
    --group=nginx \
    --with-http_ssl_module \
    --with-http_realip_module \
    --with-http_addition_module \
    --with-http_sub_module \
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_mp4_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_random_index_module \
    --with-http_secure_link_module \
    --with-http_stub_status_module \
    --with-http_auth_request_module \
    --with-threads \
    --with-stream \
    --with-stream_ssl_module \
    --with-http_slice_module \
    --with-mail \
    --with-mail_ssl_module \
    --with-file-aio \
    --with-http_v2_module \
    --with-ipv6 \
    --with-debug \
    --with-zlib=../zlib-1.2.8 \
    --with-pcre=../pcre-8.38 \
    --add-module=../nginx-upload-progress-module/ \
    --add-module=../nginx-rtmp-module/
$ make
$ sudo make install
```

安装后修改配置文件 `/etc/nginx/nginx.conf` 。

```
user exotic;
worker_processes 2;
pid /run/nginx.pid;

events {
    worker_connections 768;
    multi_accept on;
}

stream {
    upstream tcp_backend {
        server 127.0.0.1:12801;
        server 127.0.0.1:12802;
    }

    server {
        listen 6060;
        proxy_pass tcp_backend;
    }
}

http {

    ##
    # Basic Settings
    ##

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    # server_tokens off;

    # server_names_hash_bucket_size 64;
    # server_name_in_redirect off;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    ##
    # Logging Settings
    ##

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    ##
    # Gzip Settings
    ##

    gzip on;
    gzip_disable "msie6";
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    # gzip_buffers 16 8k;
    # gzip_http_version 1.1;
    gzip_types
        application/atom+xml
        application/javascript
        application/json
        application/ld+json
        application/manifest+json
        application/rss+xml
        application/vnd.geo+json
        application/vnd.ms-fontobject
        application/x-font-ttf
        application/x-web-app-manifest+json
        application/xhtml+xml
        application/xml
        font/opentype
        image/bmp
        image/svg+xml
        image/x-icon
        text/cache-manifest
        text/css
        text/plain
        text/vcard
        text/vnd.rim.location.xloc
        text/vtt
        text/x-component
        text/x-cross-domain-policy;

    ##
    # nginx-naxsi config
    ##
    # Uncomment it if you installed nginx-naxsi
    ##

    #include /etc/nginx/naxsi_core.rules;

    ##
    # nginx-passenger config
    ##
    # Uncomment it if you installed nginx-passenger
    ##

    #passenger_root /usr;
    #passenger_ruby /usr/bin/ruby;

    ##
    # Virtual Host Configs
    ##

    # include /etc/nginx/conf.d/*.conf;
    # include /etc/nginx/sites-enabled/*;

    upstream backend {
        server 127.0.0.1:12701;
        server 127.0.0.1:12702;
    }

    server {
        listen 80;

        # allow file uploading
        client_max_body_size 50M;

        location ^~ /static/ {
            root /var/www/exotic-server;
            if ($query_string) {
                expires max;
            }
        }

        location ~* /api/download/(.+) {
            root /tmp/exotic-server/;
            try_files /$1 =404;
        }

        location = /favicon.ico {
            rewrite (.*) /static/images/favicon.ico;
        }

        location /hls {
            alias /tmp/exotic-server/hls;
            autoindex on;
            autoindex_localtime on;
            set $sent_http_accept_ranges bytes;
            types {
                video/MP2T ts;
                application/vnd.apple.mpegurl m3u8;
            }
        }
		
		location /hls1 {
            alias /tmp/exotic-server/hls1;
            autoindex on;
            autoindex_localtime on;
            set $sent_http_accept_ranges bytes;
            types {
                video/MP2T ts;
                application/vnd.apple.mpegurl m3u8;
            }
        }
        location /hls2 {
            alias /tmp/exotic-server/hls2;
            autoindex on;
            autoindex_localtime on;
            set $sent_http_accept_ranges bytes;
            types {
                video/MP2T ts;
                application/vnd.apple.mpegurl m3u8;
            }
        }
        location /rtmpcontrol {
            rtmp_control all;
        }

        location /rtmpstat {
            rtmp_stat all;
            rtmp_stat_stylesheet stat.xsl;
        }

        location /socket/ {
            proxy_pass http://backend;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $http_host;
            proxy_set_header X-Scheme $scheme;
            proxy_read_timeout 300;
        }

        location / {
            proxy_pass_header Server;
            proxy_set_header Host $http_host;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Scheme $scheme;
            proxy_pass http://backend/;
        }
    }
}

rtmp {
    server {
        listen 1935;
        chunk_size 4096;
        buflen 500ms;

        application rtmp {
            live on;

            hls on;
            hls_path /tmp/exotic-server/hls;
            hls_fragment 2s;
            hls_playlist_length 30s;

            # Configure manual recording of keyframes only
            # Needs HTTP GET to get triggered
            recorder timelapse {
                record keyframes manual;
                record_path /tmp/exotic-server/live/record;
                record_suffix -keyframes.flv;
                record_unique on;
                record_interval 6h;
            }

            # Configure manual recording of all frames / live video
            recorder ondemand {
                record video manual;
                record_suffix -dump.flv;
                record_path /tmp/exotic-server/live/record;
                record_unique on;
                record_notify on;
                record_max_size 1024M;
            }

        }
        
        application rtmp1 {
 		   live on;

            hls on;
            hls_path /tmp/exotic-server/hls1;
            hls_fragment 2s;
            hls_playlist_length 30s;
		}
		application rtmp2 {
 		   live on;

            hls on;
            hls_path /tmp/exotic-server/hls2;
            hls_fragment 2s;
            hls_playlist_length 30s;
		}
    }
}
```

若需更详细的日志，可在 error_log 后添加 debug 选项。

```
error_log /var/log/nginx/error.log debug;
```

## 六、 Supervisor

```
$ sudo apt-get install supervisor
```

安装后创建配置文件 `/etc/supervisor/conf.d/exotic.conf`。

```
[program:exotic_server]
process_name=EXOTIC_SERVER_%(process_num)s
directory=/var/www/exotic-server
command=/var/www/exotic-server/env/bin/python /var/www/exotic-server/main.py --http-port=127%(process_num)02d --tcp-port=128%(process_num)02d --proxy-port-start=12501 --proxy-port-pairs=2 --proxy-port-diff=100 --config=config.py
startsecs=2
user=exotic
stdout_logfile=/var/log/exotic-server/server-%(process_num)s-stdout.log
stderr_logfile=/var/log/exotic-server/server-%(process_num)s-stderr.log
numprocs=2
numprocs_start=1
environment=DEPLOYMENT_TYPE=PRODUCTION

[program:exotic_proxy]
process_name=EXOTIC_PROXY_%(process_num)s
directory=/var/www/exotic-server
command=/var/www/exotic-server/env/bin/python /var/www/exotic-server/utils/relay.py -i 125%(process_num)02d -o 126%(process_num)02d
startsecs=1
user=exotic
stdout_logfile=/var/log/exotic-server/proxy-%(process_num)s-stdout.log
srderr_logfile=/var/log/exotic-server/proxy-%(process_num)s-stderr.log
numprocs=2
numprocs_start=1
```

重启服务使配置生效。

```
$ sudo service supervisor restart
```

## 七、 libzmq

```
$ sudo apt-get install libzmq-dev
```

安装后需要下载 commit 为 `ae44993` 的zmq/eventloop/future.py文件，地址为 https://github.com/zeromq/pyzmq/blob/master/zmq/eventloop/future.py，

https://raw.githubusercontent.com/zeromq/pyzmq/master/zmq/eventloop/future.py

并将其替换至 `.../site-packages/zmq/eventloop/future.py`。

原因为 `_AsyncSocket` 中没有定义 `close` 函数，关闭时会调用普通 `socket` 的 `close` 函数，因此无法清除 `eventloop` 中的 `handler`，从而引起严重的错误。

## 八、 生产环境

首先安装项目依赖。

```
$ sudo apt-get install mongodb
```

然后获取项目源代码。

```
$ mkdir -p /var/www
$ cd /var/www
$ git clone https://github.com/All-less/exotic-server.git
$ cd exotic-server
$ virtualenv env -p python3.5
$ env/bin/pip install -r requirements/common.txt
$ cd views
$ npm install (sudo /home/exotic/.nvm/versions/node/v6.2.0/bin/node /home/exotic/.nvm/versions/node/v6.2.0/lib/node_modules/npm)
$ npm run build	
```

然后在顶层工程目录中新建 `config.py`。

```python
# -*- coding: utf-8 -*-

smtp_host = 'smtp.exmail.qq.com'
smtp_port = 465
mail_addr = 'exotic@all-less.com'
mail_pass = 'Exotic1'

salt_pos = 17

tmp_dir = '/tmp/exotic-server'

rtmp_host = '211.101.15.234'
rtmp_port = 1935

file_url = 'http://211.101.15.234/api/download'
```

## 九、 开发环境

首先安装项目 `mongodb` 。

然后获取项目源代码。

```
$ git clone https://github.com/All-less/exotic-server.git
$ cd exotic-server
$ virtualenv env -p python3.5
$ env/bin/pip install -r requirements/common.txt
$ env/bin/pip install -r requirements/dev.txt
$ cd views
$ npm install (sudo /home/exotic/.nvm/versions/node/v6.2.0/bin/node /home/exotic/.nvm/versions/node/v6.2.0/lib/node_modules/npm)
$ npm start
```

然后在顶层工程目录中新建 `config.py`。其中典型配置如下：

```python
# -*- coding: utf-8 -*-

smtp_host = 'smtp.exmail.qq.com'
smtp_port = 465
mail_addr = 'exotic@all-less.com'
mail_pass = 'Exotic1'

salt_pos = 17

tmp_dir = '/tmp/exotic-server'

file_url = 'http://localhost:6060/api/download'
```

开发过程中，通过以下命令启动服务器：

```
$ ./main.py --config=config.py
```
$ ./utils/relay.py

若需要修改前端，则需启动 `npm` 。

```
$ cd views
$ npm start
```

## 十、 远程控制、部署

在开发环境的工程目录下，可通过 [`Fabric`](fabfile.org) 控制服务器的启动、停止及自动更新。

执行以下命令即可启动和停止服务器。其中 `<user>:<host>` 即为 SSH 所使用的信息。

```
$ env/bin/fab -H <user>@<host> start
$ env/bin/fab -H <user>@<host> stop
```

执行以下命令可控制服务器自动从 git 拉取最近的代码并重启程序。

```
$ env/bin/fab -H <user>@<host> deploy
```

# 建立rpi认证表并添加数据
db.rpi.insert({"auth_key" : "75c6b3b5a375bd52da841906f220694f", "device_id" : "device3", "hls_path" : "hls3", "live_host" : "10.214.128.192", "recv_port" : 0, "rtmp_app" : "rtmp3", "rtmp_port" : 1935, "send_port" : 0 })

db.rpi.insert({"auth_key" : "49f149a9c5600b7a2b5c67474fc73876", "device_id" : "device2", "hls_path" : "hls2", "live_host" : "10.214.128.192", "recv_port" : 0, "rtmp_app" : "rtmp2", "rtmp_port" : 1935, "send_port" : 0 })

db.rpi.insert({"auth_key" : "ba74b828469046f653e44ad954de3b7a", "device_id" : "device1", "hls_path" : "hls1", "live_host" : "10.214.128.192", "recv_port" : 0, "rtmp_app" : "rtmp1", "rtmp_port" : 1935, "send_port" : 0 })

db.rpi.insert({"auth_key" : "7aeb64ae34a82383f46472ae46644bb4", "device_id" : "device0", "hls_path" : "hls0", "live_host" : "10.214.128.192", "recv_port" : 0, "rtmp_app" : "rtmp0", "rtmp_port" : 1935, "send_port" : 0 })
