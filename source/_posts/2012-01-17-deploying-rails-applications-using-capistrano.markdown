---
layout: post
title: "Deploying Rails Applications Using Capistrano"
date: 2012-01-17 08:01
comments: true
categories: ruby rails capistrano unicorn nginx deploy
---

About [Capistrano](https://github.com/capistrano/capistrano): 

> Capistrano is a utility and framework for executing commands in parallel on multiple remote machines, via SSH. It uses a simple DSL (borrowed in part from [Rake](http://rake.rubyforge.org)) that allows you to define tasks, which may be applied to machines in certain roles. It also supports tunneling connections via some gateway machine to allow operations to be performed behind VPN's and firewalls.
>
> Capistrano was originally designed to simplify and automate deployment of web applications to distributed environments, and originally came bundled with a set of tasks designed for deploying Rails applications.

In the project that I'm working for, I'd set up Capistrano to deploy our Rails application to development environment. 

The first step is install Capistrano, using the follow command `gem install capistrano`. 

Next step is setup is Rails application for use Capistrano, and you can do it running the follow command in the root folder of your Rails application `capify .`

```
[add] writing './Capfile'
[add] writing './config/deploy.rb'
[done] capified!
```

Capfile contents:

``` ruby
load 'deploy' if respond_to?(:namespace) # cap2 differentiator

# Uncomment if you are using Rails' asset pipeline
# load 'deploy/assets'

Dir['vendor/gems/*/recipes/*.rb','vendor/plugins/*/recipes/*.rb'].each { |plugin| load(plugin) }

load 'config/deploy' # remove this line to skip loading any of the default tasks
```

And the `deploy.rb` contents:

``` ruby
set :use_sudo, false

# RVM
$:.unshift(File.expand_path('./lib', ENV['rvm_path']))
require "rvm/capistrano"
set :rvm_ruby_string, '1.9.3'

# bundle install
require "bundler/capistrano"

set :application, "yourappname"
set :repository,  "https://yourdeployuser@bitbucket.org/matheustardivo/yourappname.git"
set :user,        "yourusername"
set :password,    "yourpassword"
set :deploy_to,   "/opt/deploy/yourappname"
set :scm,         :git
set :scm_verbose, true

default_run_options[:pty] = true

# http and https proxy
default_environment['http_proxy'] = 'proxy.dev.infra:3128'
default_environment['https_proxy'] = 'proxy.dev.infra:3128'

# bypass git ssl certificate verification
default_environment['GIT_SSL_NO_VERIFY'] = 'true'

role :web, "prov-dev-1.dev.infra"
role :app, "prov-dev-1.dev.infra"
role :db,  "prov-dev-1.dev.infra", :primary => true

pid_file = "/opt/deploy/yourappname/shared/pids/unicorn.pid"
start_command = "unicorn_rails -D -E development -c /opt/deploy/yourappname/current/config/deploy/unicorn.rb"
stop_command = "kill -s QUIT `cat #{pid_file}`"
stop_condition = "if [[ -f #{pid_file} ]]; then #{stop_command}; fi"

namespace :deploy do
  task :start do
    run start_command
  end
  
  task :stop do
    run stop_condition
  end
  
  task :restart do
    run stop_condition
    sleep 2
    run start_command
  end
end
```

And finally, here is my `unicorn.rb` configuration file:

``` ruby
worker_processes 2
working_directory "/opt/deploy/yourappname/current"
listen "/opt/deploy/yourappname/shared/yourappname.socket", :backlog => 64
timeout 300
pid "/opt/deploy/yourappname/shared/pids/unicorn.pid"
stderr_path "/opt/deploy/yourappname/shared/log/unicorn.log"
stdout_path "/opt/deploy/yourappname/shared/log/unicorn.log"

```

About [Unicorn](http://unicorn.bogomips.org): 

> Unicorn is an HTTP server for Rack applications designed to only serve fast clients on low-latency, high-bandwidth connections and take advantage of features in Unix/Unix-like kernels. Slow clients should only be served by placing a reverse proxy capable of fully buffering both the the request and response in between Unicorn and slow clients.

Now that we have our configuration done, just run `cap deploy:setup` and then `cap deploy`. You will see a lot of information and at the end, your application will be deployed to your development environment. 

Other files that I use to deploy is the nginx configuration file and the nginx application configuration file. Here is the `nginx.conf`

```
user  nginx;
worker_processes  10;
worker_rlimit_nofile 100000;

error_log   /var/log/nginx/error.log;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
    use epoll;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    tcp_nopush      on;
    tcp_nodelay     on;
    server_tokens   off;
    gzip            on;
    gzip_static     on;
    gzip_comp_level 5;
    gzip_min_length 1024;
    keepalive_timeout  65;
    limit_zone   myzone  $binary_remote_addr  10m;

    # Load config files from the /etc/nginx/conf.d directory
    include /etc/nginx/conf.d/*.conf;

    server {
        limit_conn   myzone  10;
        listen       80;
        server_name  _;

        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }

        error_page  404              /404.html;
        location = /404.html {
            root   /usr/share/nginx/html;
        }

        error_page   500 502 503 504  /50x.html;
        
        location = /50x.html {
            root   /usr/share/nginx/html;
        }
    }
}

```

And the `yourappname.conf`

```
upstream yourappname {
  server unix:/opt/deploy/yourappname/shared/yourappname.socket fail_timeout=0;
}

server {
  listen 80 default deferred;
  # server_name example.com;
  root /opt/deploy/yourappname/current/public;
  try_files $uri/index.html $uri @yourappname;
  location @yourappname {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    proxy_pass http://yourappname;
  }

  error_page 400 404 500 502 503 504 http://errorpage.yourdomain.com.br;
  client_max_body_size 4G;
  keepalive_timeout 10;
}

```

Then I created symbolic links for each configuration files for nginx:

```
$ ln -s /opt/deploy/yourappname/current/config/deploy/nginx.conf /etc/nginx/nginx.conf
$ ln -s /opt/deploy/yourappname/current/config/deploy/yourappname.conf /etc/nginx/conf.d/yourappname.conf
```

That's all.
